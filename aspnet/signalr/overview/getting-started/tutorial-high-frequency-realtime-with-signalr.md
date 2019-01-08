---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutoriel : Créer des applications en temps réel haute fréquence avec SignalR 2 | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR pour fournir des fonctionnalités de messagerie à fréquence élevée.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098588"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="b0307-103">Tutoriel : Créer des applications en temps réel haute fréquence avec SignalR 2</span><span class="sxs-lookup"><span data-stu-id="b0307-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="b0307-104">Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de messagerie à fréquence élevée.</span><span class="sxs-lookup"><span data-stu-id="b0307-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="b0307-105">Dans ce cas, « échanges à fréquence élevée » signifie que le serveur envoie des mises à jour à un taux fixe.</span><span class="sxs-lookup"><span data-stu-id="b0307-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="b0307-106">Vous envoyez 10 messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="b0307-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="b0307-107">L’application que vous créez affiche une forme que les utilisateurs peuvent faire glisser.</span><span class="sxs-lookup"><span data-stu-id="b0307-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="b0307-108">Le serveur met à jour la position de la forme dans tous les navigateurs connectés pour correspondre à la position de la forme déplacée à l’aide de mises à jour a expiré.</span><span class="sxs-lookup"><span data-stu-id="b0307-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="b0307-109">Concepts présentés dans ce didacticiel ont des applications dans des jeux en temps réel et autres applications de simulation.</span><span class="sxs-lookup"><span data-stu-id="b0307-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="b0307-110">Dans ce didacticiel, vous :</span><span class="sxs-lookup"><span data-stu-id="b0307-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b0307-111">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="b0307-111">Set up the project</span></span>
> * <span data-ttu-id="b0307-112">Créer l’application de base</span><span class="sxs-lookup"><span data-stu-id="b0307-112">Create the base application</span></span>
> * <span data-ttu-id="b0307-113">Mapper vers le hub au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="b0307-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="b0307-114">Ajouter le client</span><span class="sxs-lookup"><span data-stu-id="b0307-114">Add the client</span></span>
> * <span data-ttu-id="b0307-115">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="b0307-115">Run the app</span></span>
> * <span data-ttu-id="b0307-116">Ajouter la boucle de client</span><span class="sxs-lookup"><span data-stu-id="b0307-116">Add the client loop</span></span>
> * <span data-ttu-id="b0307-117">Ajouter la boucle de serveur</span><span class="sxs-lookup"><span data-stu-id="b0307-117">Add the server loop</span></span>
> * <span data-ttu-id="b0307-118">Ajouter des animations fluides</span><span class="sxs-lookup"><span data-stu-id="b0307-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="b0307-119">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b0307-119">Prerequisites</span></span>

* <span data-ttu-id="b0307-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.</span><span class="sxs-lookup"><span data-stu-id="b0307-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="b0307-121">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="b0307-121">Set up the project</span></span>

<span data-ttu-id="b0307-122">Dans cette section, vous créez le projet dans Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="b0307-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="b0307-123">Cette section montre comment utiliser Visual Studio 2017 pour créer une Application de Web ASP.NET vide et ajouter les bibliothèques de SignalR et jQuery.UI.</span><span class="sxs-lookup"><span data-stu-id="b0307-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="b0307-124">Dans Visual Studio, créez une Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b0307-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer le web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="b0307-126">Dans le **nouvelle Application Web ASP.NET - MoveShapeDemo** fenêtre, laissez le champ **vide** sélectionné et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0307-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="b0307-127">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="b0307-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="b0307-128">Dans **ajouter un nouvel élément - MoveShapeDemo**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="b0307-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="b0307-129">Nommez la classe *MoveShapeHub* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="b0307-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="b0307-130">Cette étape crée la *MoveShapeHub.cs* fichier de classe.</span><span class="sxs-lookup"><span data-stu-id="b0307-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="b0307-131">Simultanément, il ajoute un ensemble de fichiers de script et les références d’assembly qui prennent en charge SignalR au projet.</span><span class="sxs-lookup"><span data-stu-id="b0307-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="b0307-132">Sélectionnez **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="b0307-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="b0307-133">Dans **Console du Gestionnaire de Package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b0307-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="b0307-134">La commande installe la bibliothèque jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="b0307-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="b0307-135">Vous l’utilisez pour animer la forme.</span><span class="sxs-lookup"><span data-stu-id="b0307-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="b0307-136">Dans **l’Explorateur de solutions**, développez le nœud de Scripts.</span><span class="sxs-lookup"><span data-stu-id="b0307-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Références de bibliothèque de script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="b0307-138">Bibliothèques de scripts pour jQuery, jQueryUI et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="b0307-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="b0307-139">Créer l’application de base</span><span class="sxs-lookup"><span data-stu-id="b0307-139">Create the base application</span></span>

<span data-ttu-id="b0307-140">Dans cette section, vous créez une application de navigateur.</span><span class="sxs-lookup"><span data-stu-id="b0307-140">In this section, you create a browser application.</span></span> <span data-ttu-id="b0307-141">L’application envoie l’emplacement de la forme sur le serveur lors de chaque événement mouse move.</span><span class="sxs-lookup"><span data-stu-id="b0307-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="b0307-142">Le serveur diffuse ces informations pour tous les autres clients connectés en temps réel.</span><span class="sxs-lookup"><span data-stu-id="b0307-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="b0307-143">Vous en savoir plus sur cette application dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="b0307-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="b0307-144">Ouvrez le *MoveShapeHub.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="b0307-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="b0307-145">Remplacez le code dans le *MoveShapeHub.cs* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="b0307-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="b0307-146">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="b0307-146">Save the file.</span></span>

<span data-ttu-id="b0307-147">Le `MoveShapeHub` classe est une implémentation d’un concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0307-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="b0307-148">Comme dans le [bien démarrer avec SignalR](tutorial-getting-started-with-signalr.md) didacticiel, le concentrateur a une méthode d’appeler directement les clients.</span><span class="sxs-lookup"><span data-stu-id="b0307-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="b0307-149">Dans ce cas, le client envoie un objet avec la nouvelle X et Y des coordonnées de la forme sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="b0307-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="b0307-150">Ces coordonnées obtient diffusées à tous les autres clients connectés.</span><span class="sxs-lookup"><span data-stu-id="b0307-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="b0307-151">SignalR sérialise automatiquement cet objet à l’aide de JSON.</span><span class="sxs-lookup"><span data-stu-id="b0307-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="b0307-152">L’application envoie la `ShapeModel` objet au client.</span><span class="sxs-lookup"><span data-stu-id="b0307-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="b0307-153">Il possède des membres pour stocker la position de la forme.</span><span class="sxs-lookup"><span data-stu-id="b0307-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="b0307-154">La version de l’objet sur le serveur a également un membre pour suivre les données de client sont stockées.</span><span class="sxs-lookup"><span data-stu-id="b0307-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="b0307-155">Cet objet empêche le serveur d’envoyer des données d’un client vers lui-même.</span><span class="sxs-lookup"><span data-stu-id="b0307-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="b0307-156">Ce membre utilise le `JsonIgnore` attribut pour empêcher l’application de sérialiser les données et de l’envoyer au client.</span><span class="sxs-lookup"><span data-stu-id="b0307-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="b0307-157">Mapper vers le hub au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="b0307-157">Map to the hub when app starts</span></span>

<span data-ttu-id="b0307-158">Ensuite, vous configurez le mappage au hub lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="b0307-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="b0307-159">SignalR 2, l’ajout d’une classe de démarrage OWIN crée le mappage.</span><span class="sxs-lookup"><span data-stu-id="b0307-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="b0307-160">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="b0307-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="b0307-161">Dans **ajouter un nouvel élément - MoveShapeDemo** sélectionnez **installé** > **Visual C#**   >  **Web** , puis Sélectionnez **classe de démarrage OWIN**.</span><span class="sxs-lookup"><span data-stu-id="b0307-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="b0307-162">Nommez la classe *démarrage* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0307-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="b0307-163">Remplacez le code par défaut dans le *Startup.cs* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="b0307-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="b0307-164">La classe de démarrage OWIN appelle `MapSignalR` lorsque l’application s’exécute le `Configuration` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b0307-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="b0307-165">L’application ajoute la classe de démarrage de OWIN traiter à l’aide de la `OwinStartup` attribut d’assembly.</span><span class="sxs-lookup"><span data-stu-id="b0307-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="b0307-166">Ajouter le client</span><span class="sxs-lookup"><span data-stu-id="b0307-166">Add the client</span></span>

<span data-ttu-id="b0307-167">Ajouter la page HTML pour le client.</span><span class="sxs-lookup"><span data-stu-id="b0307-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="b0307-168">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **HTML Page**.</span><span class="sxs-lookup"><span data-stu-id="b0307-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="b0307-169">Nommez la page **par défaut** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0307-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="b0307-170">Dans **l’Explorateur de solutions**, avec le bouton droit *Default.html* et sélectionnez **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="b0307-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="b0307-171">Remplacez le code par défaut dans le *Default.html* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="b0307-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="b0307-172">Dans **l’Explorateur de solutions**, développez **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="b0307-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="b0307-173">Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="b0307-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b0307-174">Le Gestionnaire de package installe une version ultérieure des scripts SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0307-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="b0307-175">Mettre à jour les références de script dans le bloc de code pour qu’elles correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="b0307-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="b0307-176">Ce code HTML et JavaScript crée une croix rouge `div` appelée `shape`.</span><span class="sxs-lookup"><span data-stu-id="b0307-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="b0307-177">Il active le comportement de glissement de la forme à l’aide de la bibliothèque jQuery et utilise le `drag` événement à la position de la forme d’envoi au serveur.</span><span class="sxs-lookup"><span data-stu-id="b0307-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b0307-178">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="b0307-178">Run the app</span></span>

<span data-ttu-id="b0307-179">Vous pouvez exécuter l’application à se'e il fonctionne.</span><span class="sxs-lookup"><span data-stu-id="b0307-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="b0307-180">Lorsque vous faites glisser la forme autour d’une fenêtre de navigateur, la forme déplace trop dans les autres navigateurs.</span><span class="sxs-lookup"><span data-stu-id="b0307-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="b0307-181">Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’application en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="b0307-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Capture d’écran de l’utilisateur sous tension en sélectionnant play et de débogage.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="b0307-183">Une fenêtre de navigateur s’ouvre avec la forme rouge dans le coin supérieur droit.</span><span class="sxs-lookup"><span data-stu-id="b0307-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="b0307-184">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="b0307-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="b0307-185">Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="b0307-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="b0307-186">Faites glisser la forme de l’une des fenêtres de navigateur.</span><span class="sxs-lookup"><span data-stu-id="b0307-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="b0307-187">La forme dans l’autre fenêtre de navigateur suit.</span><span class="sxs-lookup"><span data-stu-id="b0307-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="b0307-188">Alors que l’application les fonctions à l’aide de cette méthode, il n’est pas un modèle de programmation recommandé.</span><span class="sxs-lookup"><span data-stu-id="b0307-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="b0307-189">Il n’existe aucune limite supérieure au nombre de messages envoyés de l’obtention.</span><span class="sxs-lookup"><span data-stu-id="b0307-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="b0307-190">Par conséquent, les clients et le serveur obtient submergés par les messages et les performances se dégradent.</span><span class="sxs-lookup"><span data-stu-id="b0307-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="b0307-191">En outre, l’application affiche une animation disjoint sur le client.</span><span class="sxs-lookup"><span data-stu-id="b0307-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="b0307-192">Cette animation saccadée se produit, car la forme déplace instantanément à chaque méthode.</span><span class="sxs-lookup"><span data-stu-id="b0307-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="b0307-193">Il est préférable si la forme se déplace correctement vers chaque nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="b0307-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="b0307-194">Ensuite, vous allez apprendre à résoudre ces problèmes.</span><span class="sxs-lookup"><span data-stu-id="b0307-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="b0307-195">Ajouter la boucle de client</span><span class="sxs-lookup"><span data-stu-id="b0307-195">Add the client loop</span></span>

<span data-ttu-id="b0307-196">Envoi de l’emplacement de la forme sur chaque événement mouse move crée une quantité inutile de trafic réseau.</span><span class="sxs-lookup"><span data-stu-id="b0307-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="b0307-197">L’application doit limiter les messages à partir du client.</span><span class="sxs-lookup"><span data-stu-id="b0307-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="b0307-198">Utiliser le code javascript `setInterval` fonction pour configurer une boucle qui envoie des informations sur la nouvelle position sur le serveur à un taux fixe.</span><span class="sxs-lookup"><span data-stu-id="b0307-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="b0307-199">Cette boucle est une représentation de base d’une « boucle de jeu ».</span><span class="sxs-lookup"><span data-stu-id="b0307-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="b0307-200">C’est une fonction appelée à plusieurs reprises qui gère toutes les fonctionnalités d’un jeu.</span><span class="sxs-lookup"><span data-stu-id="b0307-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="b0307-201">Remplacez le code client dans le *Default.html* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="b0307-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="b0307-202">Vous devez remplacer les références de script à nouveau.</span><span class="sxs-lookup"><span data-stu-id="b0307-202">You have to replace the script references again.</span></span> <span data-ttu-id="b0307-203">Ils doivent correspondre les versions des scripts dans le projet.</span><span class="sxs-lookup"><span data-stu-id="b0307-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="b0307-204">Ce nouveau code ajoute la `updateServerModel` (fonction).</span><span class="sxs-lookup"><span data-stu-id="b0307-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="b0307-205">Elle est appelée sur une fréquence fixe.</span><span class="sxs-lookup"><span data-stu-id="b0307-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="b0307-206">La fonction envoie les données de la position sur le serveur chaque fois que le `moved` indicateur indique qu’il existe de nouvelles données de position à envoyer.</span><span class="sxs-lookup"><span data-stu-id="b0307-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="b0307-207">Sélectionnez le bouton lecture pour démarrer l’application</span><span class="sxs-lookup"><span data-stu-id="b0307-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="b0307-208">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="b0307-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="b0307-209">Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="b0307-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="b0307-210">Faites glisser la forme de l’une des fenêtres de navigateur.</span><span class="sxs-lookup"><span data-stu-id="b0307-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="b0307-211">La forme dans l’autre fenêtre de navigateur suit.</span><span class="sxs-lookup"><span data-stu-id="b0307-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="b0307-212">Dans la mesure où l’application limite le nombre de messages qui sont envoyés au serveur, l’animation ne s’affiche pas aussi fluide a fait dans un premier temps.</span><span class="sxs-lookup"><span data-stu-id="b0307-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="b0307-213">Ajouter la boucle de serveur</span><span class="sxs-lookup"><span data-stu-id="b0307-213">Add the server loop</span></span>

<span data-ttu-id="b0307-214">Dans l’application actuelle, les messages envoyés à partir du serveur au client accède souvent lors de leur réception.</span><span class="sxs-lookup"><span data-stu-id="b0307-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="b0307-215">Ce trafic réseau pose un problème similaire comme nous voir sur le client.</span><span class="sxs-lookup"><span data-stu-id="b0307-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="b0307-216">L’application peut envoyer des messages plus souvent qu’ils sont nécessaires.</span><span class="sxs-lookup"><span data-stu-id="b0307-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="b0307-217">La connexion peut par conséquent devenir submergée.</span><span class="sxs-lookup"><span data-stu-id="b0307-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="b0307-218">Cette section décrit comment mettre à jour le serveur pour ajouter un minuteur qui limite le taux des messages sortants.</span><span class="sxs-lookup"><span data-stu-id="b0307-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="b0307-219">Remplacez le contenu de `MoveShapeHub.cs` avec ce code :</span><span class="sxs-lookup"><span data-stu-id="b0307-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="b0307-220">Sélectionnez le bouton lecture pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="b0307-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="b0307-221">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="b0307-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="b0307-222">Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="b0307-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="b0307-223">Faites glisser la forme de l’une des fenêtres de navigateur.</span><span class="sxs-lookup"><span data-stu-id="b0307-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="b0307-224">Ce code développe le client pour ajouter la `Broadcaster` classe.</span><span class="sxs-lookup"><span data-stu-id="b0307-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="b0307-225">La nouvelle classe limite les messages sortants à l’aide de la `Timer` classe du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b0307-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="b0307-226">Il est judicieux d’apprendre que le hub lui-même est transitoire.</span><span class="sxs-lookup"><span data-stu-id="b0307-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="b0307-227">Il est créé chaque fois qu’il est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="b0307-227">It's created every time it's needed.</span></span> <span data-ttu-id="b0307-228">L’application crée le `Broadcaster` comme un singleton.</span><span class="sxs-lookup"><span data-stu-id="b0307-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="b0307-229">Elle utilise l’initialisation tardive pour différer la `Broadcaster`de la création jusqu'à ce qu’il est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="b0307-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="b0307-230">Qui garantit que l’application crée la première instance de hub complètement avant de démarrer la minuterie.</span><span class="sxs-lookup"><span data-stu-id="b0307-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="b0307-231">L’appel à les clients' `UpdateShape` fonction est ensuite déplacée hors du concentrateur `UpdateModel` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b0307-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="b0307-232">Il ne soit plus appelée dès que l’application reçoit les messages entrants.</span><span class="sxs-lookup"><span data-stu-id="b0307-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="b0307-233">Au lieu de cela, l’application envoie les messages aux clients à un débit de 25 appels par seconde.</span><span class="sxs-lookup"><span data-stu-id="b0307-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="b0307-234">Le processus est géré par le `_broadcastLoop` minuteur depuis la `Broadcaster` classe.</span><span class="sxs-lookup"><span data-stu-id="b0307-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="b0307-235">Enfin, au lieu d’appeler la méthode du client à partir du hub directement, le `Broadcaster` classe a besoin d’obtenir une référence à l’en cours d’exécution `_hubContext` hub.</span><span class="sxs-lookup"><span data-stu-id="b0307-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="b0307-236">Il obtient la référence avec le `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="b0307-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="b0307-237">Ajouter des animations fluides</span><span class="sxs-lookup"><span data-stu-id="b0307-237">Add smooth animation</span></span>

<span data-ttu-id="b0307-238">L’application est presque terminée, mais nous pourrions éventuellement effectuer une amélioration de plus.</span><span class="sxs-lookup"><span data-stu-id="b0307-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="b0307-239">L’application déplace la forme sur le client en réponse aux messages du serveur.</span><span class="sxs-lookup"><span data-stu-id="b0307-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="b0307-240">Au lieu de définir la position de la forme vers le nouvel emplacement donné par le serveur, utilisez la bibliothèque JQuery UI `animate` (fonction).</span><span class="sxs-lookup"><span data-stu-id="b0307-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="b0307-241">Il peut déplacer la forme sans heurts entre sa position actuelle et nouvelle.</span><span class="sxs-lookup"><span data-stu-id="b0307-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="b0307-242">Mettre à jour le client `updateShape` méthode dans le *Default.html* fichier ressemble le code en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="b0307-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="b0307-243">Sélectionnez le bouton lecture pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="b0307-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="b0307-244">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="b0307-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="b0307-245">Ouvrir un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="b0307-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="b0307-246">Faites glisser la forme de l’une des fenêtres de navigateur.</span><span class="sxs-lookup"><span data-stu-id="b0307-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="b0307-247">Le déplacement de la forme dans l’autre fenêtre s’affiche moins saccadé.</span><span class="sxs-lookup"><span data-stu-id="b0307-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="b0307-248">L’application effectue une interpolation son déplacement progressivement, plutôt que définie une seule fois par message entrant.</span><span class="sxs-lookup"><span data-stu-id="b0307-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="b0307-249">Ce code déplace la forme de l’ancien emplacement vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="b0307-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="b0307-250">Le serveur donne la position de la forme au cours de l’intervalle de l’animation.</span><span class="sxs-lookup"><span data-stu-id="b0307-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="b0307-251">Dans ce cas, qui est 100 millisecondes.</span><span class="sxs-lookup"><span data-stu-id="b0307-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="b0307-252">L’application efface toute animation précédente est en cours d’exécution sur la forme avant le démarrage de la nouvelle animation.</span><span class="sxs-lookup"><span data-stu-id="b0307-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0307-253">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b0307-253">Additional resources</span></span>

<span data-ttu-id="b0307-254">Le paradigme de communication que vous venez d’apprendre est utile pour le développement de jeux en ligne et autres simulations, comme [le jeu de ShootR créé avec SignalR](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="b0307-254">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="b0307-255">Pour en savoir plus sur SignalR, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="b0307-255">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="b0307-256">Projet de SignalR</span><span class="sxs-lookup"><span data-stu-id="b0307-256">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="b0307-257">SignalR GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="b0307-257">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="b0307-258">Wiki de SignalR</span><span class="sxs-lookup"><span data-stu-id="b0307-258">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="b0307-259">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="b0307-259">Next steps</span></span>

<span data-ttu-id="b0307-260">Dans ce didacticiel, vous :</span><span class="sxs-lookup"><span data-stu-id="b0307-260">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b0307-261">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="b0307-261">Set up the project</span></span>
> * <span data-ttu-id="b0307-262">Création de l’application de base</span><span class="sxs-lookup"><span data-stu-id="b0307-262">Created the base application</span></span>
> * <span data-ttu-id="b0307-263">Mappé au hub au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="b0307-263">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="b0307-264">Ajouté le client</span><span class="sxs-lookup"><span data-stu-id="b0307-264">Added the client</span></span>
> * <span data-ttu-id="b0307-265">Exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="b0307-265">Ran the app</span></span>
> * <span data-ttu-id="b0307-266">Ajouté la boucle de client</span><span class="sxs-lookup"><span data-stu-id="b0307-266">Added the client loop</span></span>
> * <span data-ttu-id="b0307-267">Ajouté la boucle de serveur</span><span class="sxs-lookup"><span data-stu-id="b0307-267">Added the server loop</span></span>
> * <span data-ttu-id="b0307-268">Ajout des animations fluides</span><span class="sxs-lookup"><span data-stu-id="b0307-268">Added smooth animation</span></span>

<span data-ttu-id="b0307-269">Passez à l’article suivant pour apprendre à créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion de serveur.</span><span class="sxs-lookup"><span data-stu-id="b0307-269">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b0307-270">SignalR 2 et la diffusion de serveur</span><span class="sxs-lookup"><span data-stu-id="b0307-270">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)