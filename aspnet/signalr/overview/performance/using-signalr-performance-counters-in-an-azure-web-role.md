---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: À l’aide des compteurs de performance SignalR dans un rôle Web Azure | Microsoft Docs
author: guardrex
description: Comment installer et utiliser des compteurs de performance SignalR dans un rôle Web Azure.
keywords: Compteur ASP.NET,SignalR,performance, rôle web azure
ms.author: riande
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 7304ff17bb53f94bdee1e90602d206bf32184e37
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795458"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="ea446-104">À l’aide des compteurs de performance SignalR dans un rôle Web Azure</span><span class="sxs-lookup"><span data-stu-id="ea446-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="ea446-105">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ea446-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ea446-106">Compteurs de performances de SignalR sont utilisés pour surveiller les performances de votre application dans un rôle Web Azure.</span><span class="sxs-lookup"><span data-stu-id="ea446-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="ea446-107">Les compteurs sont capturées par les Diagnostics Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ea446-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="ea446-108">Installation de compteurs de performance SignalR sur Azure avec *signalr.exe*, le même outil utilisé pour les applications autonomes ou en local.</span><span class="sxs-lookup"><span data-stu-id="ea446-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="ea446-109">Étant donné que les rôles Azure sont temporaires, vous configurez une application pour installer et inscrire les compteurs de performances de SignalR au démarrage.</span><span class="sxs-lookup"><span data-stu-id="ea446-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea446-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ea446-110">Prerequisites</span></span>

* <span data-ttu-id="ea446-111">Visual Studio 2015 ou [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="ea446-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="ea446-112">[Microsoft Azure SDK pour Visual Studio](https://azure.microsoft.com/downloads/) **Remarque : redémarrer votre ordinateur après avoir installé le Kit de développement.**</span><span class="sxs-lookup"><span data-stu-id="ea446-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="ea446-113">Abonnement Microsoft Azure : pour vous inscrire pour un compte d’essai Azure gratuit, consultez [essai gratuit d’Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="ea446-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="ea446-114">Création d’une application de rôle Web Azure qui expose les compteurs de performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="ea446-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="ea446-115">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea446-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="ea446-116">Dans Visual Studio, sélectionnez **fichier** > **New** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="ea446-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="ea446-117">Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C#** > **Cloud** catégorie sur la gauche, puis sélectionnez le **Azure Cloud Service** modèle.</span><span class="sxs-lookup"><span data-stu-id="ea446-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="ea446-118">Nommez l’application **SignalRPerfCounters** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea446-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nouvelle Application Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="ea446-120">Si vous ne voyez pas le **Cloud** catégorie du modèle ou la **Azure Cloud Service** modèle, vous devez installer le **développement Azure** charge de travail pour Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ea446-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="ea446-121">Choisissez le **ouvrir Visual Studio Installer** lien situé en bas à gauche de la **nouveau projet** boîte de dialogue pour ouvrir le programme d’installation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea446-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="ea446-122">Sélectionnez le **développement Azure** charge de travail, puis choisissez **modifier** pour démarrer l’installation de la charge de travail.</span><span class="sxs-lookup"><span data-stu-id="ea446-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Charge de travail de développement Azure dans le programme d’installation de Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="ea446-124">Dans le **nouveau Microsoft Azure Cloud Service** boîte de dialogue, sélectionnez **rôle Web ASP.NET** et sélectionnez le > pour ajouter le rôle pour le projet.</span><span class="sxs-lookup"><span data-stu-id="ea446-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="ea446-125">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea446-125">Select **OK**.</span></span>

   ![Ajouter le rôle Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="ea446-127">Dans le **nouvelle Application Web ASP.NET - WebRole1** boîte de dialogue, sélectionnez le **MVC** modèle, puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea446-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Ajouter MVC et l’API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="ea446-129">Dans **l’Explorateur de solutions**, ouvrez le *diagnostics.wadcfgx* de fichiers sous **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="ea446-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx de l’Explorateur de solutions](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="ea446-131">Remplacez le contenu du fichier avec la configuration suivante et enregistrez le fichier :</span><span class="sxs-lookup"><span data-stu-id="ea446-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="ea446-132">Ouvrez le **Console du Gestionnaire de Package** de **outils** > **Gestionnaire de Package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ea446-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="ea446-133">Entrez les commandes suivantes pour installer la dernière version de SignalR et le package d’utilitaires de SignalR :</span><span class="sxs-lookup"><span data-stu-id="ea446-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="ea446-134">Configurer l’application pour installer les compteurs de performance SignalR dans l’instance de rôle lorsqu’il démarre ou est recyclé.</span><span class="sxs-lookup"><span data-stu-id="ea446-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="ea446-135">Dans **l’Explorateur de solutions**, avec le bouton droit sur le **WebRole1** de projet et sélectionnez **ajouter** > **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="ea446-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="ea446-136">Nommez le nouveau dossier *démarrage*.</span><span class="sxs-lookup"><span data-stu-id="ea446-136">Name the new folder *Startup*.</span></span>

   ![Ajouter le dossier de démarrage](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="ea446-138">Copie le *signalr.exe* fichier (ajouté avec le **Microsoft.AspNet.SignalR.Utils** package) à partir de \<dossier du projet > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< version > / Outils pour le *démarrage* dossier que vous avez créé à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="ea446-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="ea446-139">Dans **l’Explorateur de solutions**, avec le bouton droit le *démarrage* dossier et sélectionnez **ajouter** > **élément existant**.</span><span class="sxs-lookup"><span data-stu-id="ea446-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="ea446-140">Dans la boîte de dialogue qui s’affiche, sélectionnez *signalr.exe* et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ea446-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Ajouter signalr.exe au projet](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="ea446-142">Avec le bouton droit sur le *démarrage* dossier que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="ea446-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="ea446-143">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ea446-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="ea446-144">Sélectionnez le **général** nœud, sélectionnez **fichier texte**et nommez le nouvel élément *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="ea446-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="ea446-145">Ce fichier de commandes installera les compteurs de performance SignalR dans le rôle web.</span><span class="sxs-lookup"><span data-stu-id="ea446-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Créer le fichier de commandes d’installation du compteur de performance SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="ea446-147">Lorsque Visual Studio crée le *SignalRPerfCounterInstall.cmd* fichier, il s’ouvre automatiquement dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="ea446-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="ea446-148">Remplacez le contenu du fichier par le script suivant, puis enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="ea446-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="ea446-149">Ce script exécute *signalr.exe*, qui ajoute les compteurs de performances de SignalR à l’instance de rôle.</span><span class="sxs-lookup"><span data-stu-id="ea446-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="ea446-150">Sélectionnez le *signalr.exe* fichier **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="ea446-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="ea446-151">Dans le fichier **propriétés**, affectez la valeur **Copy to Output Directory** à **toujours copier**.</span><span class="sxs-lookup"><span data-stu-id="ea446-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Définir la copie au répertoire de sortie pour toujours copier](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="ea446-153">Répétez l’étape précédente pour le *SignalRPerfCounterInstall.cmd* fichier.</span><span class="sxs-lookup"><span data-stu-id="ea446-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>


16. <span data-ttu-id="ea446-154">Avec le bouton droit sur le *SignalRPerfCounterInstall.cmd* fichier et sélectionnez **ouvrir avec**.</span><span class="sxs-lookup"><span data-stu-id="ea446-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="ea446-155">Dans la boîte de dialogue qui s’affiche, sélectionnez **éditeur binaire** et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea446-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Ouvrir avec l’éditeur binaire](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="ea446-157">Dans l’éditeur binaire, sélectionnez les octets de début dans le fichier, puis supprimez-les.</span><span class="sxs-lookup"><span data-stu-id="ea446-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="ea446-158">Enregistrez et fermez le fichier.</span><span class="sxs-lookup"><span data-stu-id="ea446-158">Save and close the file.</span></span>

    ![Supprimer les octets de début](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="ea446-160">Ouvrez *ServiceDefinition.csdef* et ajoutez une tâche de démarrage qui s’exécute le *SignalrPerfCounterInstall.cmd* fichier lorsque le service démarre :</span><span class="sxs-lookup"><span data-stu-id="ea446-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="ea446-161">Ouvrez `Views/Shared/_Layout.cshtml` et supprimer le script d’offre groupée de jQuery à partir de la fin du fichier.</span><span class="sxs-lookup"><span data-stu-id="ea446-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="ea446-162">Ajouter un client JavaScript qui appelle continuellement la `increment` méthode sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ea446-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="ea446-163">Ouvrez `Views/Home/Index.cshtml` et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ea446-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="ea446-164">Créer un nouveau dossier dans le **WebRole1** projet nommé *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="ea446-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="ea446-165">Avec le bouton droit le *Hubs* dossier **l’Explorateur de solutions** et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ea446-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="ea446-166">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **Web** > **SignalR** catégorie, puis sélectionnez le **classe de concentrateur SignalR (v2)** modèle d’élément.</span><span class="sxs-lookup"><span data-stu-id="ea446-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="ea446-167">Nommez le nouveau concentrateur *MyHub.cs* et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ea446-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Ajout de classe de concentrateur SignalR dans le dossier de Hubs dans la boîte de dialogue Ajouter un nouvel élément](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="ea446-169">*MyHub.cs* s’ouvre automatiquement dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="ea446-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="ea446-170">Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :</span><span class="sxs-lookup"><span data-stu-id="ea446-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="ea446-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  est une outil fourni avec le code de base SignalR de test la densité des connexions.</span><span class="sxs-lookup"><span data-stu-id="ea446-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="ea446-172">Comme manivelle requiert une connexion permanente, vous ajouté à votre site à utiliser lors du test.</span><span class="sxs-lookup"><span data-stu-id="ea446-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="ea446-173">Ajouter un nouveau dossier pour le **WebRole1** projet appelé *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="ea446-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="ea446-174">Cliquez sur ce dossier et sélectionnez **ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="ea446-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="ea446-175">Nommez le nouveau fichier de classe *MyPersistentConnections.cs* et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ea446-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="ea446-176">Visual Studio ouvre le *MyPersistentConnections.cs* fichier dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="ea446-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="ea446-177">Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :</span><span class="sxs-lookup"><span data-stu-id="ea446-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="ea446-178">À l’aide de la `Startup` (classe), les objets de SignalR démarrer au démarrage de OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea446-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="ea446-179">Ouvrez ou créez *Startup.cs* et remplacez le contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ea446-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="ea446-180">Dans le code ci-dessus, le `OwinStartup` attribut marque cette classe pour commencer à OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea446-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="ea446-181">Le `Configuration` méthode commence à SignalR.</span><span class="sxs-lookup"><span data-stu-id="ea446-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="ea446-182">Tester votre application dans l’émulateur Microsoft Azure en appuyant sur **F5**.</span><span class="sxs-lookup"><span data-stu-id="ea446-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea446-183">Si vous rencontrez un **FileLoadException** à **MapSignalR**, modifiez les redirections de liaison dans *web.config* à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="ea446-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="ea446-184">Attendez environ une minute.</span><span class="sxs-lookup"><span data-stu-id="ea446-184">Wait about one minute.</span></span> <span data-ttu-id="ea446-185">Ouvrez la fenêtre d’outil Cloud Explorer dans Visual Studio (**vue** > **Cloud Explorer**) et développez le chemin d’accès `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="ea446-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="ea446-186">Double-cliquez sur **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="ea446-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="ea446-187">Vous devez voir les compteurs SignalR dans les données de table.</span><span class="sxs-lookup"><span data-stu-id="ea446-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="ea446-188">Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="ea446-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="ea446-189">Vous devrez peut-être sélectionner la **Actualiser** bouton pour afficher la table dans **Cloud Explorer** ou sélectionnez le **Actualiser** bouton dans la fenêtre Ouvrir une table pour afficher les données de la table.</span><span class="sxs-lookup"><span data-stu-id="ea446-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Sélection de la Table de compteurs de performances WAD dans Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Affiche les compteurs collectés dans la Table des compteurs de Performance WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="ea446-192">Pour tester votre application dans le cloud, mettre à jour le **ServiceConfiguration.Cloud.cscfg** de fichiers et définir le `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` à une chaîne de connexion de compte de stockage Azure valide.</span><span class="sxs-lookup"><span data-stu-id="ea446-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="ea446-193">Déployer l’application sur votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="ea446-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="ea446-194">Pour plus d’informations sur la façon de déployer une application dans Azure, consultez [comment créer et déployer un Service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="ea446-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="ea446-195">Patientez quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="ea446-195">Wait a few minutes.</span></span> <span data-ttu-id="ea446-196">Dans **Cloud Explorer**, recherchez le compte de stockage que vous avez configurée précédemment et recherchez le `WADPerformanceCountersTable` table qu’elle contient.</span><span class="sxs-lookup"><span data-stu-id="ea446-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="ea446-197">Vous devez voir les compteurs SignalR dans les données de table.</span><span class="sxs-lookup"><span data-stu-id="ea446-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="ea446-198">Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification de stockage Azure.</span><span class="sxs-lookup"><span data-stu-id="ea446-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="ea446-199">Vous devrez peut-être sélectionner la **Actualiser** bouton pour afficher la table dans **Cloud Explorer** ou sélectionnez le **Actualiser** bouton dans la fenêtre Ouvrir une table pour afficher les données de la table.</span><span class="sxs-lookup"><span data-stu-id="ea446-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="ea446-200">Remerciements [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pour le contenu d’origine utilisé dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ea446-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
