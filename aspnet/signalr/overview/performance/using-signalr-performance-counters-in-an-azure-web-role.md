---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: À l’aide des compteurs de performance SignalR dans un rôle Web Azure | Documents Microsoft
author: guardrex
description: Comment installer et utiliser des compteurs de performance SignalR dans un rôle Web Azure.
keywords: Compteur ASP.NET,SignalR,performance, rôle web azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2018
ms.locfileid: "28988014"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="732fd-104">À l’aide des compteurs de performance SignalR dans un rôle Web Azure</span><span class="sxs-lookup"><span data-stu-id="732fd-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="732fd-105">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="732fd-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="732fd-106">Compteurs de performance SignalR permettent de surveiller les performances de votre application dans un rôle Web Azure.</span><span class="sxs-lookup"><span data-stu-id="732fd-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="732fd-107">Les compteurs sont capturées par les Diagnostics Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="732fd-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="732fd-108">Vous installez des compteurs de performance SignalR sur Azure avec *signalr.exe*, le même outil utilisé pour les applications autonomes ou en local.</span><span class="sxs-lookup"><span data-stu-id="732fd-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="732fd-109">Étant donné que les rôles Windows Azure sont temporaires, vous configurez une application pour installer et inscrire les compteurs de performance SignalR lors du démarrage.</span><span class="sxs-lookup"><span data-stu-id="732fd-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="732fd-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="732fd-110">Prerequisites</span></span>

* [<span data-ttu-id="732fd-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="732fd-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="732fd-112">[Microsoft Azure SDK pour Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Remarque : redémarrer votre ordinateur après avoir installé le Kit de développement.**</span><span class="sxs-lookup"><span data-stu-id="732fd-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="732fd-113">Abonnement Microsoft Azure : pour vous inscrire pour un compte d’essai Azure, consultez [essai gratuit d’Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="732fd-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="732fd-114">Création d’une application de rôle Web Azure qui expose les compteurs de performance SignalR</span><span class="sxs-lookup"><span data-stu-id="732fd-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="732fd-115">Ouvrez Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="732fd-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="732fd-116">Dans Visual Studio 2015, sélectionnez **fichier** > **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="732fd-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="732fd-117">Dans le **modèles** volet de la **nouveau projet** fenêtre sous le **Visual C#** nœud, sélectionnez le **Cloud** nœud et sélectionnez le **Azure Cloud Service** modèle.</span><span class="sxs-lookup"><span data-stu-id="732fd-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="732fd-118">Nom de l’application **SignalRPerfCounters** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="732fd-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nouvelle Application de Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="732fd-120">Dans le **nouveau Microsoft Azure Cloud Service** boîte de dialogue, sélectionnez **rôle Web ASP.NET** et sélectionnez le > bouton pour ajouter le rôle pour le projet.</span><span class="sxs-lookup"><span data-stu-id="732fd-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="732fd-121">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="732fd-121">Select **OK**.</span></span>

   ![Ajouter le rôle Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="732fd-123">Dans le **nouvelle Application Web ASP.NET - WebRole1** boîte de dialogue, sélectionnez le **MVC** modèle, puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="732fd-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Ajouter MVC et l’API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="732fd-125">Dans **l’Explorateur de solutions**, ouvrez le *diagnostics.wadcfgx* de fichiers sous **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="732fd-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx de l’Explorateur de solutions](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="732fd-127">Remplacez le contenu du fichier avec la configuration suivante, puis enregistrez le fichier :</span><span class="sxs-lookup"><span data-stu-id="732fd-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="732fd-128">Ouvrez le **Package Manager Console** de **outils** > **Gestionnaire de Package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="732fd-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="732fd-129">Entrez les commandes suivantes pour installer la dernière version de SignalR et le package d’utilitaires SignalR :</span><span class="sxs-lookup"><span data-stu-id="732fd-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="732fd-130">Configurer l’application pour installer les compteurs de performance SignalR dans l’instance de rôle lorsqu’il démarre ou est recyclé.</span><span class="sxs-lookup"><span data-stu-id="732fd-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="732fd-131">Dans **l’Explorateur de solutions**, avec le bouton droit sur le **WebRole1** de projet et sélectionnez **ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="732fd-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="732fd-132">Nommez le nouveau dossier *démarrage*.</span><span class="sxs-lookup"><span data-stu-id="732fd-132">Name the new folder *Startup*.</span></span>

   ![Ajouter un dossier de démarrage](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="732fd-134">Copie le *signalr.exe* fichier (ajoutée avec la **Microsoft.AspNet.SignalR.Utils** package) à partir de \<dossier du projet > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< version > / Outils pour le *démarrage* dossier que vous avez créé à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="732fd-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="732fd-135">Dans **l’Explorateur de solutions**, avec le bouton droit le *démarrage* et sélectionnez **ajouter** > **élément existant**.</span><span class="sxs-lookup"><span data-stu-id="732fd-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="732fd-136">Dans la boîte de dialogue qui s’affiche, sélectionnez *signalr.exe* et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="732fd-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Ajouter signalr.exe au projet](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="732fd-138">Avec le bouton droit sur le *démarrage* dossier que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="732fd-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="732fd-139">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="732fd-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="732fd-140">Sélectionnez le **général** nœud, sélectionnez **fichier texte**et nommez le nouvel élément *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="732fd-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="732fd-141">Ce fichier de commande installe les compteurs de performance SignalR dans le rôle web.</span><span class="sxs-lookup"><span data-stu-id="732fd-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Créer le fichier de commandes d’installation du compteur de performance SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="732fd-143">Lorsque Visual Studio crée le *SignalRPerfCounterInstall.cmd* fichier, il s’ouvre automatiquement dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="732fd-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="732fd-144">Remplacez le contenu du fichier par le script suivant, puis enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="732fd-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="732fd-145">Ce script s’exécute *signalr.exe*, qui ajoute les compteurs de performance SignalR à l’instance de rôle.</span><span class="sxs-lookup"><span data-stu-id="732fd-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="732fd-146">Sélectionnez le *signalr.exe* fichier **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="732fd-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="732fd-147">Dans du fichier **propriétés**, définissez **copier dans le répertoire de sortie** à **toujours copier**.</span><span class="sxs-lookup"><span data-stu-id="732fd-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Affectez à copier vers le répertoire de sortie pour toujours copier](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="732fd-149">Répétez l’étape précédente pour le *SignalRPerfCounterInstall.cmd* fichier.</span><span class="sxs-lookup"><span data-stu-id="732fd-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="732fd-150">Avec le bouton droit sur le *SignalRPerfCounterInstall.cmd* fichier et sélectionnez **ouvrir avec**.</span><span class="sxs-lookup"><span data-stu-id="732fd-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="732fd-151">Dans la boîte de dialogue qui s’affiche, sélectionnez **éditeur binaire** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="732fd-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Ouvrir avec l’éditeur binaire](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="732fd-153">Dans l’éditeur binaire, sélectionnez tous les octets au début du fichier et les supprimer.</span><span class="sxs-lookup"><span data-stu-id="732fd-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="732fd-154">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="732fd-154">Save and close the file.</span></span>

    ![Supprimer des octets au début](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="732fd-156">Ouvrez *ServiceDefinition.csdef* et ajoutez une tâche de démarrage qui s’exécute le *SignalrPerfCounterInstall.cmd* fichier lorsque le service démarre :</span><span class="sxs-lookup"><span data-stu-id="732fd-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="732fd-157">Ouvrez `Views/Shared/_Layout.cshtml` et supprimer le script de lot jQuery à partir de la fin du fichier.</span><span class="sxs-lookup"><span data-stu-id="732fd-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="732fd-158">Ajouter un client JavaScript qui appelle continuellement la `increment` méthode sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="732fd-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="732fd-159">Ouvrez `Views/Home/Index.cshtml` et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="732fd-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="732fd-160">Créer un nouveau dossier dans le **WebRole1** projet nommé *concentrateurs*.</span><span class="sxs-lookup"><span data-stu-id="732fd-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="732fd-161">Avec le bouton droit le *concentrateurs* dossier **l’Explorateur de solutions**, sélectionnez **Web** > **SignalR**, puis sélectionnez  **Classe de concentrateur SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="732fd-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="732fd-162">Nommez le nouveau concentrateur *MyHub.cs* et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="732fd-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Ajouter une classe concentrateur SignalR pour le dossier de concentrateurs dans la boîte de dialogue Ajouter un nouvel élément](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="732fd-164">*MyHub.cs* s’ouvre automatiquement dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="732fd-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="732fd-165">Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :</span><span class="sxs-lookup"><span data-stu-id="732fd-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="732fd-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  est un outil fourni avec le code base SignalR de test de densité de connexion.</span><span class="sxs-lookup"><span data-stu-id="732fd-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="732fd-167">Puisque manivelle requiert une connexion permanente, vous ajoutez une à votre site à utiliser lors du test.</span><span class="sxs-lookup"><span data-stu-id="732fd-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="732fd-168">Ajouter un nouveau dossier pour le **WebRole1** projet appelé *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="732fd-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="732fd-169">Cliquez sur ce dossier et sélectionnez **ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="732fd-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="732fd-170">Nommez le nouveau fichier de classe *MyPersistentConnections.cs* et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="732fd-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="732fd-171">Visual Studio ouvre le *MyPersistentConnections.cs* fichier dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="732fd-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="732fd-172">Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :</span><span class="sxs-lookup"><span data-stu-id="732fd-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="732fd-173">À l’aide de la `Startup` (classe), les objets SignalR démarrer au démarrage de OWIN.</span><span class="sxs-lookup"><span data-stu-id="732fd-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="732fd-174">Ouvrez ou créez *Startup.cs* et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="732fd-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="732fd-175">Dans le code ci-dessus, le `OwinStartup` attribut marque cette classe pour OWIN.</span><span class="sxs-lookup"><span data-stu-id="732fd-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="732fd-176">Le `Configuration` méthode commence à SignalR.</span><span class="sxs-lookup"><span data-stu-id="732fd-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="732fd-177">Testez votre application dans l’émulateur Microsoft Azure en appuyant sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="732fd-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="732fd-178">Si vous rencontrez un **FileLoadException** à **MapSignalR**, modifiez les redirections de liaison dans *web.config* à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="732fd-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="732fd-179">Attendez environ une minute.</span><span class="sxs-lookup"><span data-stu-id="732fd-179">Wait about one minute.</span></span> <span data-ttu-id="732fd-180">Ouvrez la fenêtre d’outil Cloud Explorer dans Visual Studio (**vue** > **Cloud Explorer**) et développez le chemin d’accès `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="732fd-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="732fd-181">Double-cliquez sur **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="732fd-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="732fd-182">Vous devez voir les compteurs SignalR dans les données de table.</span><span class="sxs-lookup"><span data-stu-id="732fd-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="732fd-183">Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="732fd-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="732fd-184">Vous devrez peut-être sélectionner la **Actualiser** bouton pour afficher la table de **Cloud Explorer** ou sélectionnez le **Actualiser** dans la fenêtre Ouvrir une table pour afficher les données de la table.</span><span class="sxs-lookup"><span data-stu-id="732fd-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Sélection de la Table de compteurs de Performance de Diagnostics Windows AZURE dans Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Affichant les compteurs collectés dans la Table de compteurs de Performance de Diagnostics Windows AZURE](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="732fd-187">Pour tester votre application dans le cloud, mettre à jour le **ServiceConfiguration.Cloud.cscfg** de fichiers et de définir le `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` à une chaîne de connexion de compte Azure Storage valide.</span><span class="sxs-lookup"><span data-stu-id="732fd-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="732fd-188">Déployez l’application à votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="732fd-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="732fd-189">Pour plus d’informations sur la façon de déployer une application dans Azure, consultez [comment créer et déployer un Service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="732fd-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="732fd-190">Attendez quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="732fd-190">Wait a few minutes.</span></span> <span data-ttu-id="732fd-191">Dans **Cloud Explorer**, recherchez le compte de stockage que vous avez configuré ci-dessus et recherchez le `WADPerformanceCountersTable` table qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="732fd-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="732fd-192">Vous devez voir les compteurs SignalR dans les données de table.</span><span class="sxs-lookup"><span data-stu-id="732fd-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="732fd-193">Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="732fd-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="732fd-194">Vous devrez peut-être sélectionner la **Actualiser** bouton pour afficher la table de **Cloud Explorer** ou sélectionnez le **Actualiser** dans la fenêtre Ouvrir une table pour afficher les données de la table.</span><span class="sxs-lookup"><span data-stu-id="732fd-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="732fd-195">Remerciements [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pour le contenu d’origine utilisé dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="732fd-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
