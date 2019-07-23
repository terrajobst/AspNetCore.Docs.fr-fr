---
title: Bien démarrer avec ASP.NET Core SignalR
author: bradygaster
description: Dans ce tutoriel, vous créez une application de conversation qui utilise ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: 53d3763a93cc72b6bcf85b64a706500299b3597f
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67893749"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="87348-103">Tutoriel : Bien démarrer avec ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="87348-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="87348-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="87348-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="87348-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="87348-106">Create a web project.</span></span>
> * <span data-ttu-id="87348-107">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="87348-108">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="87348-109">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="87348-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="87348-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="87348-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="87348-111">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="87348-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="87348-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="87348-117">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="87348-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="87348-119">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="87348-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="87348-120">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="87348-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="87348-121">Dans la boîte de dialogue **Configurer votre nouveau projet**, sélectionnez *SignalRChat*, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="87348-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="87348-122">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, sélectionnez **.NET Core** et **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="87348-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="87348-123">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="87348-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="87348-126">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="87348-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="87348-127">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="87348-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-128">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="87348-129">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="87348-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="87348-130">Sélectionnez **.NET Core > Application > Application web** (ne sélectionnez pas **Application web (modèle-vue-contrôleur)** ), puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="87348-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="87348-131">Veillez à sélectionner **.NET Core 3.0** comme **Framework cible**, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="87348-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="87348-132">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="87348-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="87348-133">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-133">Add the SignalR client library</span></span>

<span data-ttu-id="87348-134">La bibliothèque de serveur SignalR est incluse dans le framework partagé ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="87348-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="87348-135">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="87348-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="87348-136">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="87348-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="87348-137">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="87348-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="87348-139">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="87348-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="87348-140">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="87348-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="87348-141">Pour **Bibliothèque**, entrez `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="87348-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="87348-142">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="87348-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="87348-143">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/* , puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="87348-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/3.x/libman1.png)

  <span data-ttu-id="87348-145">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="87348-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="87348-147">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="87348-148">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="87348-149">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="87348-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="87348-150">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="87348-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="87348-151">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="87348-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="87348-152">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="87348-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="87348-153">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="87348-153">Copy only the specified files.</span></span>

  <span data-ttu-id="87348-154">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="87348-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-155">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="87348-156">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="87348-157">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="87348-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="87348-158">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="87348-159">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="87348-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="87348-160">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="87348-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="87348-161">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="87348-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="87348-162">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="87348-162">Copy only the specified files.</span></span>

  <span data-ttu-id="87348-163">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="87348-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="87348-164">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-164">Create a SignalR hub</span></span>

<span data-ttu-id="87348-165">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="87348-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="87348-166">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="87348-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="87348-167">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="87348-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="87348-168">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="87348-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="87348-169">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="87348-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="87348-170">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="87348-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="87348-171">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="87348-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="87348-172">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="87348-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="87348-173">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-173">Configure SignalR</span></span>

<span data-ttu-id="87348-174">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="87348-175">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="87348-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="87348-176">Ces modifications ajoutent SignalR aux systèmes de routage et d’injection de dépendances ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87348-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="87348-177">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-177">Add SignalR client code</span></span>

* <span data-ttu-id="87348-178">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="87348-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="87348-179">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="87348-179">The preceding code:</span></span>

  * <span data-ttu-id="87348-180">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="87348-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="87348-181">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="87348-182">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="87348-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="87348-183">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="87348-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="87348-184">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="87348-184">The preceding code:</span></span>

  * <span data-ttu-id="87348-185">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="87348-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="87348-186">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="87348-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="87348-187">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="87348-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="87348-188">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="87348-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="87348-190">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="87348-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="87348-192">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="87348-192">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-193">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="87348-194">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="87348-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="87348-195">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="87348-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="87348-196">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="87348-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="87348-197">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="87348-197">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="87348-199">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="87348-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="87348-200">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87348-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="87348-201">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="87348-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="87348-202">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="87348-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="87348-203">![Erreur de fichier SignalR.js introuvable](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="87348-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="87348-204">Si vous obtenez l’erreur ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY dans Chrome ou NS_ERROR_NET_INADEQUATE_SECURITY dans Firefox, exécutez ces commandes pour mettre à jour votre certificat de développement :</span><span class="sxs-lookup"><span data-stu-id="87348-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="87348-205">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="87348-205">Next steps</span></span>

<span data-ttu-id="87348-206">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="87348-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="87348-207">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="87348-208">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="87348-209">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="87348-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="87348-210">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="87348-210">Create a web project.</span></span>
> * <span data-ttu-id="87348-211">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="87348-212">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="87348-213">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="87348-214">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="87348-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="87348-215">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="87348-215">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="87348-217">Prérequis</span><span class="sxs-lookup"><span data-stu-id="87348-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-220">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="87348-221">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="87348-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="87348-223">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="87348-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="87348-224">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="87348-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="87348-225">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="87348-225">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="87348-227">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="87348-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="87348-228">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.2**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="87348-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="87348-231">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="87348-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="87348-232">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="87348-232">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-233">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="87348-234">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="87348-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="87348-235">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="87348-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="87348-236">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="87348-236">Select **Next**.</span></span>

* <span data-ttu-id="87348-237">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="87348-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="87348-238">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-238">Add the SignalR client library</span></span>

<span data-ttu-id="87348-239">La bibliothèque de serveur SignalR est incluse dans le métapackage `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="87348-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="87348-240">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="87348-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="87348-241">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="87348-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="87348-242">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="87348-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="87348-244">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="87348-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="87348-245">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="87348-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="87348-246">Pour **Bibliothèque**, entrez `@aspnet/signalr@1`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="87348-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="87348-248">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="87348-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="87348-249">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/* , puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="87348-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="87348-251">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="87348-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="87348-253">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="87348-254">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="87348-255">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="87348-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="87348-256">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="87348-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="87348-257">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="87348-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="87348-258">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="87348-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="87348-259">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="87348-259">Copy only the specified files.</span></span>

  <span data-ttu-id="87348-260">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="87348-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-261">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="87348-262">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="87348-263">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="87348-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="87348-264">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="87348-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="87348-265">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="87348-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="87348-266">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="87348-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="87348-267">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="87348-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="87348-268">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="87348-268">Copy only the specified files.</span></span>

  <span data-ttu-id="87348-269">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="87348-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="87348-270">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-270">Create a SignalR hub</span></span>

<span data-ttu-id="87348-271">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="87348-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="87348-272">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="87348-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="87348-273">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="87348-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="87348-274">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="87348-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="87348-275">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="87348-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="87348-276">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="87348-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="87348-277">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="87348-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="87348-278">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="87348-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="87348-279">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-279">Configure SignalR</span></span>

<span data-ttu-id="87348-280">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="87348-281">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="87348-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="87348-282">Ces modifications ajoutent SignalR au système d’injection de dépendances ASP.NET Core et au pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="87348-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="87348-283">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-283">Add SignalR client code</span></span>

* <span data-ttu-id="87348-284">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="87348-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="87348-285">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="87348-285">The preceding code:</span></span>

  * <span data-ttu-id="87348-286">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="87348-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="87348-287">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="87348-288">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="87348-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="87348-289">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="87348-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="87348-290">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="87348-290">The preceding code:</span></span>

  * <span data-ttu-id="87348-291">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="87348-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="87348-292">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="87348-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="87348-293">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="87348-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="87348-294">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="87348-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="87348-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87348-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="87348-296">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="87348-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="87348-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87348-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="87348-298">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="87348-298">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="87348-299">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="87348-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="87348-300">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="87348-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="87348-301">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="87348-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="87348-302">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="87348-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="87348-303">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="87348-303">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="87348-305">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="87348-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="87348-306">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="87348-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="87348-307">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="87348-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="87348-308">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="87348-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="87348-309">![Erreur de fichier SignalR.js introuvable](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="87348-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="87348-310">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="87348-310">Next steps</span></span>

<span data-ttu-id="87348-311">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="87348-311">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="87348-312">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="87348-312">Create a web app project.</span></span>
> * <span data-ttu-id="87348-313">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-313">Add the SignalR client library.</span></span>
> * <span data-ttu-id="87348-314">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-314">Create a SignalR hub.</span></span>
> * <span data-ttu-id="87348-315">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="87348-315">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="87348-316">Ajouter le code qui utilise le hub pour envoyer des messages de n’importe quel client à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="87348-316">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="87348-317">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="87348-317">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="87348-318">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="87348-318">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end
