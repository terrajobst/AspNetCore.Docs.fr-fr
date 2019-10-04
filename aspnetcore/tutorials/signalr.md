---
title: Bien démarrer avec ASP.NET Core SignalR
author: bradygaster
description: Dans ce tutoriel, vous créez une application de conversation qui utilise ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 09/24/2019
uid: tutorials/signalr
ms.openlocfilehash: bec01adc2682f83b0225df66e221bd2e4ea9feb4
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925312"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="42624-103">Tutoriel : Bien démarrer avec ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="42624-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="42624-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="42624-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42624-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="42624-106">Create a web project.</span></span>
> * <span data-ttu-id="42624-107">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="42624-108">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="42624-109">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="42624-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="42624-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="42624-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="42624-111">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="42624-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="42624-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="42624-117">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="42624-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="42624-119">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="42624-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="42624-120">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="42624-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="42624-121">Dans la boîte de dialogue **Configurer votre nouveau projet**, sélectionnez *SignalRChat*, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="42624-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="42624-122">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, sélectionnez **.NET Core** et **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="42624-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="42624-123">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="42624-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="42624-126">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="42624-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="42624-127">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="42624-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-128">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="42624-129">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="42624-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="42624-130">Sélectionnez **.NET Core > Application > Application web** (ne sélectionnez pas **Application web (modèle-vue-contrôleur)** ), puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="42624-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="42624-131">Veillez à sélectionner **.NET Core 3.0** comme **Framework cible**, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="42624-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="42624-132">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="42624-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="42624-133">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-133">Add the SignalR client library</span></span>

<span data-ttu-id="42624-134">La bibliothèque de serveur SignalR est incluse dans le framework partagé ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="42624-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="42624-135">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="42624-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="42624-136">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="42624-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="42624-137">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="42624-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="42624-139">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="42624-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="42624-140">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="42624-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="42624-141">Pour **Bibliothèque**, entrez `@microsoft/signalr@latest`.</span><span class="sxs-lookup"><span data-stu-id="42624-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="42624-142">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="42624-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="42624-143">Définissez **emplacement cible** sur *wwwroot/js/signalr/* , puis sélectionnez **installer**.</span><span class="sxs-lookup"><span data-stu-id="42624-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="42624-145">LibMan crée un dossier *wwwroot/js/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="42624-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="42624-147">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="42624-148">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="42624-149">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="42624-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="42624-150">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="42624-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="42624-151">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="42624-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="42624-152">Copiez les fichiers vers la destination *wwwroot/js/signalr* .</span><span class="sxs-lookup"><span data-stu-id="42624-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="42624-153">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="42624-153">Copy only the specified files.</span></span>

  <span data-ttu-id="42624-154">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="42624-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-155">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="42624-156">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="42624-157">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="42624-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="42624-158">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="42624-159">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="42624-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="42624-160">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="42624-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="42624-161">Copiez les fichiers vers la destination *wwwroot/js/signalr* .</span><span class="sxs-lookup"><span data-stu-id="42624-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="42624-162">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="42624-162">Copy only the specified files.</span></span>

  <span data-ttu-id="42624-163">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="42624-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="42624-164">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-164">Create a SignalR hub</span></span>

<span data-ttu-id="42624-165">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="42624-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="42624-166">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="42624-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="42624-167">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="42624-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="42624-168">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="42624-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="42624-169">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="42624-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="42624-170">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="42624-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="42624-171">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="42624-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="42624-172">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="42624-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="42624-173">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-173">Configure SignalR</span></span>

<span data-ttu-id="42624-174">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="42624-175">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="42624-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="42624-176">Ces modifications ajoutent SignalR aux systèmes de routage et d’injection de dépendances ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42624-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="42624-177">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-177">Add SignalR client code</span></span>

* <span data-ttu-id="42624-178">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="42624-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="42624-179">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="42624-179">The preceding code:</span></span>

  * <span data-ttu-id="42624-180">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="42624-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="42624-181">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="42624-182">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="42624-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="42624-183">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="42624-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="42624-184">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="42624-184">The preceding code:</span></span>

  * <span data-ttu-id="42624-185">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="42624-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="42624-186">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="42624-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="42624-187">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="42624-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="42624-188">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="42624-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="42624-190">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="42624-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="42624-192">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="42624-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-193">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="42624-194">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="42624-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="42624-195">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="42624-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="42624-196">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="42624-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="42624-197">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="42624-197">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="42624-199">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="42624-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="42624-200">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="42624-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="42624-201">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="42624-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="42624-202">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="42624-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="42624-203">![Erreur de fichier SignalR.js introuvable](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="42624-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="42624-204">Si vous recevez l’erreur ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY dans Chrome, exécutez les commandes suivantes pour mettre à jour votre certificat de développement :</span><span class="sxs-lookup"><span data-stu-id="42624-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="42624-205">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="42624-205">Next steps</span></span>

<span data-ttu-id="42624-206">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="42624-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="42624-207">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="42624-208">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="42624-209">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="42624-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42624-210">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="42624-210">Create a web project.</span></span>
> * <span data-ttu-id="42624-211">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="42624-212">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="42624-213">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="42624-214">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="42624-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="42624-215">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="42624-215">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="42624-217">Prérequis</span><span class="sxs-lookup"><span data-stu-id="42624-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-220">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="42624-221">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="42624-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="42624-223">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="42624-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="42624-224">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="42624-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="42624-225">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="42624-225">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="42624-227">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="42624-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="42624-228">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.2**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="42624-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="42624-231">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="42624-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="42624-232">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="42624-232">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-233">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="42624-234">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="42624-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="42624-235">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="42624-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="42624-236">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="42624-236">Select **Next**.</span></span>

* <span data-ttu-id="42624-237">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="42624-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="42624-238">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-238">Add the SignalR client library</span></span>

<span data-ttu-id="42624-239">La bibliothèque de serveur SignalR est incluse dans le métapackage `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="42624-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="42624-240">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="42624-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="42624-241">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="42624-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="42624-242">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="42624-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="42624-244">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="42624-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="42624-245">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="42624-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="42624-246">Pour **Bibliothèque**, entrez `@aspnet/signalr@1`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="42624-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="42624-248">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="42624-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="42624-249">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/* , puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="42624-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="42624-251">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="42624-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="42624-253">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="42624-254">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="42624-255">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="42624-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="42624-256">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="42624-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="42624-257">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="42624-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="42624-258">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="42624-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="42624-259">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="42624-259">Copy only the specified files.</span></span>

  <span data-ttu-id="42624-260">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="42624-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-261">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="42624-262">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="42624-263">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="42624-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="42624-264">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="42624-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="42624-265">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="42624-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="42624-266">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="42624-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="42624-267">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="42624-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="42624-268">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="42624-268">Copy only the specified files.</span></span>

  <span data-ttu-id="42624-269">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="42624-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="42624-270">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-270">Create a SignalR hub</span></span>

<span data-ttu-id="42624-271">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="42624-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="42624-272">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="42624-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="42624-273">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="42624-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="42624-274">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="42624-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="42624-275">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="42624-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="42624-276">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="42624-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="42624-277">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="42624-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="42624-278">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="42624-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="42624-279">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-279">Configure SignalR</span></span>

<span data-ttu-id="42624-280">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="42624-281">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="42624-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="42624-282">Ces modifications ajoutent SignalR au système d’injection de dépendances ASP.NET Core et au pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="42624-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="42624-283">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-283">Add SignalR client code</span></span>

* <span data-ttu-id="42624-284">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="42624-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="42624-285">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="42624-285">The preceding code:</span></span>

  * <span data-ttu-id="42624-286">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="42624-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="42624-287">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="42624-288">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="42624-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="42624-289">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="42624-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="42624-290">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="42624-290">The preceding code:</span></span>

  * <span data-ttu-id="42624-291">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="42624-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="42624-292">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="42624-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="42624-293">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="42624-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="42624-294">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="42624-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42624-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42624-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="42624-296">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="42624-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42624-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42624-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="42624-298">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="42624-298">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="42624-299">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="42624-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="42624-300">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="42624-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="42624-301">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="42624-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="42624-302">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="42624-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="42624-303">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="42624-303">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="42624-305">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="42624-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="42624-306">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="42624-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="42624-307">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="42624-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="42624-308">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="42624-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="42624-309">![Erreur de fichier SignalR.js introuvable](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="42624-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42624-310">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="42624-310">Additional resources</span></span>

* [<span data-ttu-id="42624-311">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="42624-311">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)

## <a name="next-steps"></a><span data-ttu-id="42624-312">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="42624-312">Next steps</span></span>

<span data-ttu-id="42624-313">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="42624-313">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42624-314">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="42624-314">Create a web app project.</span></span>
> * <span data-ttu-id="42624-315">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-315">Add the SignalR client library.</span></span>
> * <span data-ttu-id="42624-316">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-316">Create a SignalR hub.</span></span>
> * <span data-ttu-id="42624-317">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="42624-317">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="42624-318">Ajouter le code qui utilise le hub pour envoyer des messages de n’importe quel client à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="42624-318">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="42624-319">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="42624-319">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="42624-320">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="42624-320">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end
