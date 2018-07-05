---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Héberger OWIN dans un rôle Worker Azure | Microsoft Docs
author: MikeWasson
description: Ce didacticiel montre comment l’auto-hébergement OWIN dans un rôle de travail Microsoft Azure. Open Web Interface pour .NET (OWIN) définit une abstraction entre le serveur web .NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 56019ef7bdf1c3e9a769ba43f624ef3c6a5e6d4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366703"
---
<a name="host-owin-in-an-azure-worker-role"></a>Héberger OWIN dans un rôle de travail Azure
====================
par [Mike Wasson](https://github.com/MikeWasson)

> Ce didacticiel montre comment l’auto-hébergement OWIN dans un rôle de travail Microsoft Azure.
> 
> [Open Web Interface pour .NET](http://owin.org/) (OWIN) définit une abstraction entre les serveurs web de .NET et des applications web. OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS – par exemple, à l’intérieur d’un rôle de travail Azure.
> 
> Dans ce didacticiel, vous allez apprendre à l’hébergement automatique d’une application OWIN à l’intérieur d’un rôle de travail Microsoft Azure. Pour en savoir plus sur les rôles de travail, consultez [modèles d’exécution Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK pour .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Créer un projet Microsoft Azure

Démarrez Visual Studio avec des privilèges d’administrateur. Des privilèges d’administrateur sont nécessaires pour déboguer l’application localement, à l’aide de l’émulateur de calcul Azure.

Sur le **fichier** menu, cliquez sur **New**, puis cliquez sur **projet**. À partir de **modèles installés**, sous Visual c#, cliquez sur **Cloud** puis cliquez sur **Windows Azure Cloud Service**. Nommez le projet « AzureApp » et cliquez sur **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

Dans le **nouveau Windows Azure Cloud Service** boîte de dialogue, double-cliquez sur **rôle de travail**. Laissez le nom par défaut (« WorkerRole1 »). Cette étape ajoute un rôle de travail à la solution. Cliquez sur **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

La solution Visual Studio qui est créée contient deux projets :

- &quot;AzureApp&quot; définit les rôles et la configuration de l’application Azure.
- &quot;WorkerRole1&quot; contient le code pour le rôle de travail.

En règle générale, une application Azure peut contenir plusieurs rôles, bien que ce didacticiel utilise un seul rôle.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Ajoutez les Packages d’auto-hébergement OWIN

À partir de la **outils** menu, cliquez sur **Library Package Manager**, puis cliquez sur **Console du Gestionnaire de Package**.

Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Ajouter un point de terminaison HTTP

Dans l’Explorateur de solutions, développez le projet AzureApp. Développez le nœud rôles, cliquez sur WorkerRole1 et sélectionnez **propriétés**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Cliquez sur **points de terminaison**, puis cliquez sur **ajouter le point de terminaison**.

Dans le **protocole** liste déroulante, sélectionnez « http ». Dans **Port Public** et **Port privé**, tapez 80. Ces numéros de port peut être différent. Le port public est ce que les clients utilisent lorsqu’ils envoient une demande au rôle.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Créer la classe de démarrage OWIN

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `Startup`.

Remplacez tout le code réutilisable avec les éléments suivants :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

Le `UseWelcomePage` méthode d’extension ajoute une page HTML simple à votre application, afin de vérifier le site fonctionne.

## <a name="start-the-owin-host"></a>Démarrer l’hôte OWIN

Ouvrez le fichier WorkerRole.cs. Cette classe définit le code qui s’exécute lorsque le rôle de travail est démarré et arrêté.

Ajoutez le code suivant à l’aide d’instruction :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Ajouter un **IDisposable** membre à la `WorkerRole` classe :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

Dans le `OnStart` (méthode), ajoutez le code suivant pour démarrer l’ordinateur hôte :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

Le **WebApp.Start** méthode commence à l’hôte OWIN. Le nom de la `Startup` classe est un paramètre de type à la méthode. Par convention, l’hôte appelle le `Configure` méthode de cette classe.

Remplacer le `OnStop` pour les supprimer de la  *\_application* instance :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Voici le code complet pour WorkerRole.cs :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Générez la solution, puis appuyez sur F5 pour exécuter l’application localement dans l’émulateur de calcul Azure. En fonction de vos paramètres de pare-feu, vous devrez peut-être autoriser l’émulateur via votre pare-feu.

L’émulateur de calcul attribue une adresse IP locale au point de terminaison. Vous pouvez trouver l’adresse IP en affichant l’interface de l’émulateur de calcul. Cliquez sur l’icône de l’émulateur dans la zone de notification de la barre des tâches, puis sélectionnez **Show Compute Emulator UI**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Recherchez l’adresse IP sous déploiements de Service, déploiement [id], détails du Service. Ouvrez un navigateur web et accédez à http://<em>adresse</em>, où <em>adresse</em> est l’adresse IP affectée par l’émulateur de calcul ; par exemple, `http://127.0.0.1:80`. Vous devez voir la page d’accueil OWIN :

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Déployer sur Azure

Pour cette étape, vous devez disposer d’un compte Azure. Si vous n’en avez déjà, vous pouvez créer un compte d’essai gratuit en quelques minutes. Pour plus d’informations, consultez [version d’évaluation gratuite de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Dans l’Explorateur de solutions, cliquez sur le projet AzureApp. Sélectionnez **Publier**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Si vous n’êtes pas connecté à votre compte Azure, cliquez sur **Sign In**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Une fois que vous êtes connecté, choisissez un abonnement, puis cliquez sur **suivant**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Entrez un nom pour le service cloud et choisissez une région. Cliquez sur **Créer**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Cliquez sur **Publier**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

La fenêtre de journal d’activité Azure affiche la progression du déploiement. Quand l’application est déployée, accédez à `http://appname.cloudapp.net/`, où *appname* est le nom de votre service cloud.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Vue d’ensemble du projet Katana](an-overview-of-project-katana.md)
- [Projet Katana sur GitHub](https://github.com/aspnet/AspNetKatana/)
