---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: À l’aide des compteurs de performance SignalR dans un rôle Web Azure | Microsoft Docs
author: guardrex
description: Comment installer et utiliser des compteurs de performance SignalR dans un rôle Web Azure.
keywords: Compteur ASP.NET,SignalR,performance, rôle web azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 8e17e945bc144731dd149bd7ddfc9e29160eaf0b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836481"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>À l’aide des compteurs de performance SignalR dans un rôle Web Azure

Par [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Compteurs de performances de SignalR sont utilisés pour surveiller les performances de votre application dans un rôle Web Azure. Les compteurs sont capturées par les Diagnostics Microsoft Azure. Installation de compteurs de performance SignalR sur Azure avec *signalr.exe*, le même outil utilisé pour les applications autonomes ou en local. Étant donné que les rôles Azure sont temporaires, vous configurez une application pour installer et inscrire les compteurs de performances de SignalR au démarrage.

## <a name="prerequisites"></a>Prérequis

* Visual Studio 2015 ou [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK pour Visual Studio](https://azure.microsoft.com/downloads/) **Remarque : Redémarrez votre ordinateur après avoir installé le Kit de développement.**
* Abonnement Microsoft Azure : Pour vous inscrire pour un compte d’essai Azure gratuit, consultez [essai gratuit d’Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Création d’une application de rôle Web Azure qui expose les compteurs de performances de SignalR

1. Ouvrez Visual Studio.

2. Dans Visual Studio, sélectionnez **Fichier** > **Nouveau** > **Projet**.

3. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C#** > **Cloud** catégorie sur la gauche, puis sélectionnez le **Azure Cloud Service** modèle. Nommez l’application **SignalRPerfCounters** et sélectionnez **OK**.

   ![Nouvelle Application Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Si vous ne voyez pas le **Cloud** catégorie du modèle ou la **Azure Cloud Service** modèle, vous devez installer le **développement Azure** charge de travail pour Visual Studio 2017. Choisissez le **ouvrir Visual Studio Installer** lien situé en bas à gauche de la **nouveau projet** boîte de dialogue pour ouvrir le programme d’installation de Visual Studio. Sélectionnez le **développement Azure** charge de travail, puis choisissez **modifier** pour démarrer l’installation de la charge de travail.
   >
   > ![Charge de travail de développement Azure dans le programme d’installation de Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. Dans le **nouveau Microsoft Azure Cloud Service** boîte de dialogue, sélectionnez **rôle Web ASP.NET** et sélectionnez le > pour ajouter le rôle pour le projet. Sélectionnez **OK**.

   ![Ajouter le rôle Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. Dans le **nouvelle Application Web ASP.NET - WebRole1** boîte de dialogue, sélectionnez le **MVC** modèle, puis sélectionnez **OK**.

   ![Ajouter MVC et l’API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. Dans **l’Explorateur de solutions**, ouvrez le *diagnostics.wadcfgx* de fichiers sous **WebRole1**.

   ![Diagnostics.wadcfgx de l’Explorateur de solutions](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Remplacez le contenu du fichier avec la configuration suivante et enregistrez le fichier :

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Ouvrez le **Console du Gestionnaire de Package** de **outils** > **Gestionnaire de Package NuGet**. Entrez les commandes suivantes pour installer la dernière version de SignalR et le package d’utilitaires de SignalR :

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Configurer l’application pour installer les compteurs de performance SignalR dans l’instance de rôle lorsqu’il démarre ou est recyclé. Dans **l’Explorateur de solutions**, avec le bouton droit sur le **WebRole1** de projet et sélectionnez **ajouter** > **nouveau dossier**. Nommez le nouveau dossier *démarrage*.

   ![Ajouter le dossier de démarrage](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Copie le *signalr.exe* fichier (ajouté avec le **Microsoft.AspNet.SignalR.Utils** package) à partir de \<dossier du projet > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< version > / Outils pour le *démarrage* dossier que vous avez créé à l’étape précédente.

11. Dans **l’Explorateur de solutions**, avec le bouton droit le *démarrage* dossier et sélectionnez **ajouter** > **élément existant**. Dans la boîte de dialogue qui s’affiche, sélectionnez *signalr.exe* et sélectionnez **ajouter**.

    ![Ajouter signalr.exe au projet](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Avec le bouton droit sur le *démarrage* dossier que vous avez créé. Sélectionnez **Ajouter** > **Nouvel élément**. Sélectionnez le **général** nœud, sélectionnez **fichier texte**et nommez le nouvel élément *SignalRPerfCounterInstall.cmd*. Ce fichier de commandes installera les compteurs de performance SignalR dans le rôle web.

    ![Créer le fichier de commandes d’installation du compteur de performance SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Lorsque Visual Studio crée le *SignalRPerfCounterInstall.cmd* fichier, il s’ouvre automatiquement dans la fenêtre principale. Remplacez le contenu du fichier par le script suivant, puis enregistrez et fermez le fichier. Ce script exécute *signalr.exe*, qui ajoute les compteurs de performances de SignalR à l’instance de rôle.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Sélectionnez le *signalr.exe* fichier **l’Explorateur de solutions**. Dans le fichier **propriétés**, affectez la valeur **Copy to Output Directory** à **toujours copier**.

    ![Définir la copie au répertoire de sortie pour toujours copier](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Répétez l’étape précédente pour le *SignalRPerfCounterInstall.cmd* fichier.


16. Avec le bouton droit sur le *SignalRPerfCounterInstall.cmd* fichier et sélectionnez **ouvrir avec**. Dans la boîte de dialogue qui s’affiche, sélectionnez **éditeur binaire** et sélectionnez **OK**.

    ![Ouvrir avec l’éditeur binaire](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. Dans l’éditeur binaire, sélectionnez les octets de début dans le fichier, puis supprimez-les. Enregistrez et fermez le fichier.

    ![Supprimer les octets de début](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Ouvrez *ServiceDefinition.csdef* et ajoutez une tâche de démarrage qui s’exécute le *SignalrPerfCounterInstall.cmd* fichier lorsque le service démarre :

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Ouvrez `Views/Shared/_Layout.cshtml` et supprimer le script d’offre groupée de jQuery à partir de la fin du fichier.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Ajouter un client JavaScript qui appelle continuellement la `increment` méthode sur le serveur. Ouvrez `Views/Home/Index.cshtml` et remplacez le contenu par le code suivant :

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Créer un nouveau dossier dans le **WebRole1** projet nommé *Hubs*. Avec le bouton droit le *Hubs* dossier **l’Explorateur de solutions** et sélectionnez **ajouter** > **un nouvel élément**. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **Web** > **SignalR** catégorie, puis sélectionnez le **classe de concentrateur SignalR (v2)** modèle d’élément. Nommez le nouveau concentrateur *MyHub.cs* et sélectionnez **ajouter**.

    ![Ajout de classe de concentrateur SignalR dans le dossier de Hubs dans la boîte de dialogue Ajouter un nouvel élément](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* s’ouvre automatiquement dans la fenêtre principale. Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  est une outil fourni avec le code de base SignalR de test la densité des connexions. Comme manivelle requiert une connexion permanente, vous ajouté à votre site à utiliser lors du test. Ajouter un nouveau dossier pour le **WebRole1** projet appelé *PersistentConnections*. Cliquez sur ce dossier et sélectionnez **ajouter** > **classe**. Nommez le nouveau fichier de classe *MyPersistentConnections.cs* et sélectionnez **ajouter**.

24. Visual Studio ouvre le *MyPersistentConnections.cs* fichier dans la fenêtre principale. Remplacez le contenu par le code suivant, puis enregistrez et fermez le fichier :

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. À l’aide de la `Startup` (classe), les objets de SignalR démarrer au démarrage de OWIN. Ouvrez ou créez *Startup.cs* et remplacez le contenu par le code suivant :

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    Dans le code ci-dessus, le `OwinStartup` attribut marque cette classe pour commencer à OWIN. Le `Configuration` méthode commence à SignalR.

26. Tester votre application dans l’émulateur Microsoft Azure en appuyant sur **F5**.

    > [!NOTE]
    > Si vous rencontrez un **FileLoadException** à **MapSignalR**, modifiez les redirections de liaison dans *web.config* à ce qui suit :

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Attendez environ une minute. Ouvrez la fenêtre d’outil Cloud Explorer dans Visual Studio (**vue** > **Cloud Explorer**) et développez le chemin d’accès `(Local)/Storage Accounts/(Development)/Tables`. Double-cliquez sur **WADPerformanceCountersTable**. Vous devez voir les compteurs SignalR dans les données de table. Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification de stockage Azure. Vous devrez peut-être sélectionner la **Actualiser** bouton pour afficher la table dans **Cloud Explorer** ou sélectionnez le **Actualiser** bouton dans la fenêtre Ouvrir une table pour afficher les données de la table.

    ![Sélection de la Table de compteurs de performances WAD dans Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Affiche les compteurs collectés dans la Table des compteurs de Performance WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Pour tester votre application dans le cloud, mettre à jour le **ServiceConfiguration.Cloud.cscfg** de fichiers et définir le `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` à une chaîne de connexion de compte de stockage Azure valide.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Déployer l’application sur votre abonnement Azure. Pour plus d’informations sur la façon de déployer une application dans Azure, consultez [comment créer et déployer un Service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Patientez quelques minutes. Dans **Cloud Explorer**, recherchez le compte de stockage que vous avez configurée précédemment et recherchez le `WADPerformanceCountersTable` table qu’elle contient. Vous devez voir les compteurs SignalR dans les données de table. Si vous ne voyez pas la table, vous devrez peut-être entrer à nouveau vos informations d’identification de stockage Azure. Vous devrez peut-être sélectionner la **Actualiser** bouton pour afficher la table dans **Cloud Explorer** ou sélectionnez le **Actualiser** bouton dans la fenêtre Ouvrir une table pour afficher les données de la table.

Remerciements [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pour le contenu d’origine utilisé dans ce didacticiel.
