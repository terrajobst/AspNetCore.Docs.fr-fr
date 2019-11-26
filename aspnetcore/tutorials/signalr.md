---
title: Prise en main de ASP.NET Core SignalR
author: bradygaster
description: Dans ce didacticiel, vous allez créer une application de conversation qui utilise ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr
ms.openlocfilehash: 55ebdbfa4556deca74a6cdf0638307425cd1a01a
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317498"
---
# <a name="tutorial-get-started-with-aspnet-core-opno-locsignalr"></a><span data-ttu-id="8b67b-103">Didacticiel : prise en main de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8b67b-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8b67b-104">Ce didacticiel enseigne les bases de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="8b67b-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="8b67b-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8b67b-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="8b67b-106">Create a web project.</span></span>
> * <span data-ttu-id="8b67b-107">Ajoutez la bibliothèque cliente SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="8b67b-108">Créez un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="8b67b-109">Configurez le projet pour qu’il utilise SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="8b67b-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="8b67b-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="8b67b-111">At the end, you'll have a working chat app:</span></span>

![[! Opérationnel. Exemple d’application non-LOC (Signalr)]](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="8b67b-113">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="8b67b-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="8b67b-117">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="8b67b-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="8b67b-119">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="8b67b-120">Dans la boîte de dialogue **Créer un projet**, sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="8b67b-121">Dans la boîte de dialogue **Configurer votre nouveau projet**, sélectionnez *SignalRChat*, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="8b67b-122">Dans la boîte de dialogue **créer une application web ASP.net Core** , sélectionnez **.net Core** et **ASP.net Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-122">In the **Create a new ASP.NET Core web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="8b67b-123">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="8b67b-126">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="8b67b-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="8b67b-127">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="8b67b-127">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-128">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8b67b-129">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="8b67b-130">Sélectionnez **.NET Core > Application > Application web** (ne sélectionnez pas **Application web (modèle-vue-contrôleur)** ), puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="8b67b-131">Veillez à sélectionner **.NET Core 3.0** comme **Framework cible**, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="8b67b-132">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="8b67b-133">Ajouter la bibliothèque cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="8b67b-133">Add the SignalR client library</span></span>

<span data-ttu-id="8b67b-134">La bibliothèque SignalR Server est incluse dans l’infrastructure partagée ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="8b67b-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="8b67b-135">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="8b67b-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="8b67b-136">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="8b67b-137">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="8b67b-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="8b67b-139">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="8b67b-140">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="8b67b-141">Pour **Bibliothèque**, entrez `@microsoft/signalr@latest`.</span><span class="sxs-lookup"><span data-stu-id="8b67b-141">For **Library**, enter `@microsoft/signalr@latest`.</span></span>

* <span data-ttu-id="8b67b-142">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="8b67b-143">Définissez **emplacement cible** sur *wwwroot/js/signalr/* , puis sélectionnez **installer**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-143">Set **Target Location** to *wwwroot/js/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  <span data-ttu-id="8b67b-145">LibMan crée un dossier *wwwroot/js/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-145">LibMan creates a *wwwroot/js/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="8b67b-147">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="8b67b-148">Exécutez la commande suivante pour récupérer la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="8b67b-149">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="8b67b-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="8b67b-150">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="8b67b-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="8b67b-151">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="8b67b-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="8b67b-152">Copiez les fichiers vers la destination *wwwroot/js/signalr* .</span><span class="sxs-lookup"><span data-stu-id="8b67b-152">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="8b67b-153">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-153">Copy only the specified files.</span></span>

  <span data-ttu-id="8b67b-154">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="8b67b-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-155">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8b67b-156">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="8b67b-157">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="8b67b-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="8b67b-158">Exécutez la commande suivante pour récupérer la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="8b67b-159">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="8b67b-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="8b67b-160">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="8b67b-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="8b67b-161">Copiez les fichiers vers la destination *wwwroot/js/signalr* .</span><span class="sxs-lookup"><span data-stu-id="8b67b-161">Copy files to the *wwwroot/js/signalr* destination.</span></span>
  * <span data-ttu-id="8b67b-162">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-162">Copy only the specified files.</span></span>

  <span data-ttu-id="8b67b-163">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="8b67b-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="8b67b-164">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="8b67b-164">Create a SignalR hub</span></span>

<span data-ttu-id="8b67b-165">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="8b67b-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="8b67b-166">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="8b67b-167">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b67b-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="8b67b-168">La classe `ChatHub` hérite de la classe `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="8b67b-169">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="8b67b-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="8b67b-170">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="8b67b-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="8b67b-171">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="8b67b-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> SignalR<span data-ttu-id="8b67b-172"> code est asynchrone pour fournir une évolutivité maximale.</span><span class="sxs-lookup"><span data-stu-id="8b67b-172"> code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="8b67b-173">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="8b67b-173">Configure SignalR</span></span>

<span data-ttu-id="8b67b-174">Le serveur de SignalR doit être configuré pour transmettre les demandes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="8b67b-175">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  <span data-ttu-id="8b67b-176">Ces modifications ajoutent des SignalR aux systèmes de routage et d’injection de dépendances ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b67b-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="8b67b-177">Ajouter SignalR code client</span><span class="sxs-lookup"><span data-stu-id="8b67b-177">Add SignalR client code</span></span>

* <span data-ttu-id="8b67b-178">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b67b-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="8b67b-179">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="8b67b-179">The preceding code:</span></span>

  * <span data-ttu-id="8b67b-180">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="8b67b-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="8b67b-181">Crée une liste avec `id="messagesList"` pour l’affichage des messages reçus à partir du concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="8b67b-182">Contient des références de script à SignalR et au code d’application de *conversation. js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="8b67b-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="8b67b-183">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b67b-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="8b67b-184">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="8b67b-184">The preceding code:</span></span>

  * <span data-ttu-id="8b67b-185">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="8b67b-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="8b67b-186">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="8b67b-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="8b67b-187">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="8b67b-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="8b67b-188">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="8b67b-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8b67b-190">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="8b67b-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8b67b-192">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8b67b-192">In the integrated terminal, run the following command:</span></span>

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-193">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8b67b-194">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="8b67b-195">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="8b67b-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="8b67b-196">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="8b67b-197">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="8b67b-197">The name and message are displayed on both pages instantly.</span></span>

  ![[! Opérationnel. Exemple d’application non-LOC (Signalr)]](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="8b67b-199">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="8b67b-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="8b67b-200">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8b67b-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="8b67b-201">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="8b67b-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="8b67b-202">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="8b67b-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="8b67b-203">![Erreur de fichier SignalR.js introuvable](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="8b67b-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="8b67b-204">Si vous recevez l’erreur ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY dans Chrome, exécutez les commandes suivantes pour mettre à jour votre certificat de développement :</span><span class="sxs-lookup"><span data-stu-id="8b67b-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome, run these commands to update your development certificate:</span></span>
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8b67b-205">Ce didacticiel enseigne les bases de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-205">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="8b67b-206">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="8b67b-206">You learn how to:</span></span> 

> [!div class="checklist"]  
> * <span data-ttu-id="8b67b-207">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="8b67b-207">Create a web project.</span></span>   
> * <span data-ttu-id="8b67b-208">Ajoutez la bibliothèque cliente SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-208">Add the SignalR client library.</span></span>   
> * <span data-ttu-id="8b67b-209">Créez un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-209">Create a SignalR hub.</span></span> 
> * <span data-ttu-id="8b67b-210">Configurez le projet pour qu’il utilise SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-210">Configure the project to use SignalR.</span></span> 
> * <span data-ttu-id="8b67b-211">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-211">Add code that sends messages from any client to all connected clients.</span></span>  
<span data-ttu-id="8b67b-212">À la fin, vous disposerez d’une application de conversation active : ![[ ! Opérationnel. NO-LOC (Signalr)] exemple d’application](signalr/_static/2.x/signalr-get-started-finished.png)</span><span class="sxs-lookup"><span data-stu-id="8b67b-212">At the end, you'll have a working chat app: ![SignalR sample app](signalr/_static/2.x/signalr-get-started-finished.png)</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="8b67b-213">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="8b67b-213">Prerequisites</span></span>    

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-214">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-214">Visual Studio</span></span>](#tab/visual-studio)   

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)] 

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-215">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-215">Visual Studio Code</span></span>](#tab/visual-studio-code) 

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]    

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-216">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-216">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]    

--- 

## <a name="create-a-web-project"></a><span data-ttu-id="8b67b-217">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="8b67b-217">Create a web project</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-218">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="8b67b-219">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-219">From the menu, select **File > New Project**.</span></span> 

* <span data-ttu-id="8b67b-220">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-220">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8b67b-221">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-221">Name the project *SignalRChat*.</span></span> 

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)    

* <span data-ttu-id="8b67b-223">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8b67b-223">Select **Web Application** to create a project that uses Razor Pages.</span></span> 

* <span data-ttu-id="8b67b-224">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.2**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-224">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>    

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-226">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-226">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="8b67b-227">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="8b67b-227">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>  

* <span data-ttu-id="8b67b-228">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="8b67b-228">Run the following commands:</span></span>   

   ```dotnetcli 
   dotnet new webapp -o SignalRChat 
   code -r SignalRChat  
   ```  

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-229">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-229">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="8b67b-230">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-230">From the menu, select **File > New Solution**.</span></span>    

* <span data-ttu-id="8b67b-231">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="8b67b-231">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>  

* <span data-ttu-id="8b67b-232">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-232">Select **Next**.</span></span>  

* <span data-ttu-id="8b67b-233">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-233">Name the project *SignalRChat*, and then select **Create**.</span></span>   

--- 

## <a name="add-the-opno-locsignalr-client-library"></a><span data-ttu-id="8b67b-234">Ajouter la bibliothèque cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="8b67b-234">Add the SignalR client library</span></span> 

<span data-ttu-id="8b67b-235">La bibliothèque SignalR Server est incluse dans le sous-package `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="8b67b-235">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="8b67b-236">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="8b67b-236">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="8b67b-237">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-237">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="8b67b-238">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="8b67b-238">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-239">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-239">Visual Studio</span></span>](#tab/visual-studio/)  

* <span data-ttu-id="8b67b-240">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-240">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>  

* <span data-ttu-id="8b67b-241">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-241">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="8b67b-242">Pour **Bibliothèque**, entrez `@microsoft/signalr@3`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="8b67b-242">For **Library**, enter `@microsoft/signalr@3`, and select the latest version that isn't preview.</span></span>  

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/2.x/libman1.png)   

* <span data-ttu-id="8b67b-244">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-244">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span> 

* <span data-ttu-id="8b67b-245">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/* , puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-245">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>    

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/2.x/libman2.png) 

  <span data-ttu-id="8b67b-247">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-247">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>    

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-248">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-248">Visual Studio Code</span></span>](#tab/visual-studio-code/)    

* <span data-ttu-id="8b67b-249">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-249">In the integrated terminal, run the following command to install LibMan.</span></span>  

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="8b67b-250">Exécutez la commande suivante pour récupérer la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-250">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="8b67b-251">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="8b67b-251">You might have to wait a few seconds before seeing output.</span></span> 

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  <span data-ttu-id="8b67b-252">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="8b67b-252">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="8b67b-253">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="8b67b-253">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="8b67b-254">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-254">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="8b67b-255">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-255">Copy only the specified files.</span></span>  

  <span data-ttu-id="8b67b-256">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="8b67b-256">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.0.1" to "wwwroot/lib/signalr" 
  ```   

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-257">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)   

* <span data-ttu-id="8b67b-258">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-258">In the **Terminal**, run the following command to install LibMan.</span></span> 

  ```dotnetcli  
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli   
  ```   

* <span data-ttu-id="8b67b-259">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="8b67b-259">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span> 

* <span data-ttu-id="8b67b-260">Exécutez la commande suivante pour récupérer la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="8b67b-260">Run the following command to get the SignalR client library by using LibMan.</span></span>    

  ```console    
  libman install @microsoft/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js 
  ```   

  <span data-ttu-id="8b67b-261">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="8b67b-261">The parameters specify the following options:</span></span> 
  * <span data-ttu-id="8b67b-262">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="8b67b-262">Use the unpkg provider.</span></span> 
  * <span data-ttu-id="8b67b-263">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-263">Copy files to the *wwwroot/lib/signalr* destination.</span></span>    
  * <span data-ttu-id="8b67b-264">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="8b67b-264">Copy only the specified files.</span></span>  

  <span data-ttu-id="8b67b-265">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="8b67b-265">The output looks like the following example:</span></span>  

  ```console    
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk   
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk   
  Installed library "@microsoft/signalr@3.x.x" to "wwwroot/lib/signalr" 
  ```   

--- 

## <a name="create-a-opno-locsignalr-hub"></a><span data-ttu-id="8b67b-266">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="8b67b-266">Create a SignalR hub</span></span>   

<span data-ttu-id="8b67b-267">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="8b67b-267">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>   

* <span data-ttu-id="8b67b-268">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-268">In the SignalRChat project folder, create a *Hubs* folder.</span></span>    

* <span data-ttu-id="8b67b-269">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b67b-269">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span> 

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]   

  <span data-ttu-id="8b67b-270">La classe `ChatHub` hérite de la classe `Hub` SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-270">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="8b67b-271">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="8b67b-271">The `Hub` class manages connections, groups, and messaging.</span></span>  

  <span data-ttu-id="8b67b-272">La méthode `SendMessage` peut être appelée par un client connecté afin d’envoyer un message à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="8b67b-272">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="8b67b-273">Le code client JavaScript qui appelle la méthode est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="8b67b-273">JavaScript client code that calls the method is shown later in the tutorial.</span></span> SignalR<span data-ttu-id="8b67b-274"> code est asynchrone pour fournir une évolutivité maximale.</span><span class="sxs-lookup"><span data-stu-id="8b67b-274"> code is asynchronous to provide maximum scalability.</span></span>    

## <a name="configure-opno-locsignalr"></a><span data-ttu-id="8b67b-275">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="8b67b-275">Configure SignalR</span></span>  

<span data-ttu-id="8b67b-276">Le serveur de SignalR doit être configuré pour transmettre les demandes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-276">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>    

* <span data-ttu-id="8b67b-277">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b67b-277">Add the following highlighted code to the *Startup.cs* file.</span></span>  

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]  

  <span data-ttu-id="8b67b-278">Ces modifications ajoutent des SignalR au système d’injection de dépendances ASP.NET Core et au pipeline d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="8b67b-278">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>  

## <a name="add-opno-locsignalr-client-code"></a><span data-ttu-id="8b67b-279">Ajouter SignalR code client</span><span class="sxs-lookup"><span data-stu-id="8b67b-279">Add SignalR client code</span></span>    

* <span data-ttu-id="8b67b-280">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b67b-280">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>  

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]   

  <span data-ttu-id="8b67b-281">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="8b67b-281">The preceding code:</span></span>   

  * <span data-ttu-id="8b67b-282">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="8b67b-282">Creates text boxes for name and message text, and a submit button.</span></span>  
  * <span data-ttu-id="8b67b-283">Crée une liste avec `id="messagesList"` pour l’affichage des messages reçus à partir du concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b67b-283">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>   
  * <span data-ttu-id="8b67b-284">Contient des références de script à SignalR et au code d’application de *conversation. js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="8b67b-284">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>    

* <span data-ttu-id="8b67b-285">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b67b-285">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>  

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]    

  <span data-ttu-id="8b67b-286">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="8b67b-286">The preceding code:</span></span>   

  * <span data-ttu-id="8b67b-287">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="8b67b-287">Creates and starts a connection.</span></span>    
  * <span data-ttu-id="8b67b-288">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="8b67b-288">Adds to the submit button a handler that sends messages to the hub.</span></span> 
  * <span data-ttu-id="8b67b-289">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="8b67b-289">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>  

## <a name="run-the-app"></a><span data-ttu-id="8b67b-290">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="8b67b-290">Run the app</span></span>  

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b67b-291">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b67b-291">Visual Studio</span></span>](#tab/visual-studio)   

* <span data-ttu-id="8b67b-292">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="8b67b-292">Press **CTRL+F5** to run the app without debugging.</span></span>   

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b67b-293">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b67b-293">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="8b67b-294">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8b67b-294">In the integrated terminal, run the following command:</span></span>    

  ```dotnetcli
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8b67b-295">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="8b67b-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8b67b-296">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-296">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="8b67b-297">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="8b67b-297">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="8b67b-298">Choisissez un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer le message**.</span><span class="sxs-lookup"><span data-stu-id="8b67b-298">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>  

  <span data-ttu-id="8b67b-299">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="8b67b-299">The name and message are displayed on both pages instantly.</span></span>   

  ![[! Opérationnel. Exemple d’application non-LOC (Signalr)]](signalr/_static/2.x/signalr-get-started-finished.png) 

> [!TIP]    
> <span data-ttu-id="8b67b-301">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="8b67b-301">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="8b67b-302">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8b67b-302">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="8b67b-303">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="8b67b-303">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="8b67b-304">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="8b67b-304">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>   
> <span data-ttu-id="8b67b-305">![Erreur de fichier SignalR.js introuvable](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="8b67b-305">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>    
## <a name="additional-resources"></a><span data-ttu-id="8b67b-306">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8b67b-306">Additional resources</span></span> 
* [<span data-ttu-id="8b67b-307">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="8b67b-307">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=iKlVmu-r0JQ)   

::: moniker-end
