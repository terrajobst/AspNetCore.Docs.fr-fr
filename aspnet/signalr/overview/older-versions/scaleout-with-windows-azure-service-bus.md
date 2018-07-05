---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Montée en puissance parallèle de SignalR avec Azure Service Bus (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: bcaff2f371c757bc819bc44b9b3bd296b085a688
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380031"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Montée en puissance parallèle de SignalR avec Azure Service Bus (SignalR 1.x)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Dans ce didacticiel, vous allez déployer une application de SignalR pour un rôle Web Windows Azure à l’aide de l’infrastructure d’intégration Service Bus pour distribuer les messages à chaque instance de rôle.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Conditions préalables :

- Un compte Windows Azure.
- Le [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Le fond de panier de bus de service est également compatible avec [Service Bus pour Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1. Toutefois, il n’est pas compatible avec la version 1.0 de Service Bus pour Windows Server.

## <a name="pricing"></a>Tarification

Le fond de panier de Service Bus utilise les rubriques pour envoyer des messages. Pour les informations de tarification les plus récentes, consultez [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Au moment de cet article est rédigé, vous pouvez envoyer 1 000 000 messages par mois pour moins de 1 $. Le fond de panier envoie un message de bus de service pour chaque appel d’une méthode de concentrateur SignalR. Il existe également des messages de contrôle pour les connexions, déconnexions, joindre ou quitter groupes et ainsi de suite. Dans la plupart des applications, la majorité du trafic message seront des appels de méthode de concentrateur.

## <a name="overview"></a>Vue d'ensemble

Avant de passer au didacticiel détaillé, Voici un aperçu rapide de la procédure à suivre.

1. Utilisez le portail Windows Azure pour créer un nouvel espace de noms Service Bus.
2. Ajoutez ces packages NuGet à votre application : 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Créez une application de SignalR.
4. Ajoutez le code suivant à global.asax de façon à configurer le fond de panier : 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Pour chaque application, choisissez une autre valeur pour « Nomapp ». N’utilisez pas la même valeur dans plusieurs applications.

## <a name="create-the-azure-services"></a>Créer les Services Azure

Créer un Service Cloud, comme décrit dans [comment créer et déployer un Service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Suivez les étapes décrites dans la section « Comment : créer un service cloud à l’aide de la création rapide ». Pour ce didacticiel, vous n’avez pas besoin de télécharger un certificat.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Créez un nouvel espace de noms Service Bus, comme décrit dans [comment faire pour utiliser rubriques/abonnements Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Suivez les étapes décrites dans la section « Créer un Namespace de Service ».

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Veillez à sélectionner la même région pour le service cloud et l’espace de noms Service Bus.


## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Démarrez Visual Studio. À partir de la **fichier** menu, cliquez sur **nouveau projet**.

Dans le **nouveau projet** boîte de dialogue, développez **Visual C#**. Sous **modèles installés**, sélectionnez **Cloud** , puis sélectionnez **Windows Azure Cloud Service**. Conservez la valeur par défaut .NET Framework 4.5. Nommez l’application « ChatService » et cliquez sur **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

Dans le **nouveau Windows Azure Cloud Service** boîte de dialogue Sélectionner un rôle ASP.NET MVC 4 Web. Cliquez sur le bouton de flèche droite (**&gt;**) pour ajouter le rôle à votre solution.

Pointez la souris sur le nouveau rôle, par conséquent, l’icône de crayon visible. Cliquez sur cette icône pour renommer le rôle. Nom du rôle « SignalRChat » et cliquez sur **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Dans le **nouveau projet ASP.NET MVC 4** Assistant, sélectionnez **Application Internet**. Cliquez sur **OK**. L’Assistant de projet crée deux projets :

- « ChatService » : Ce projet est l’application Windows Azure. Il définit les rôles Azure et autres options de configuration.
- SignalRChat : Ce projet est votre projet ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Créer l’Application de conversation de SignalR

Pour créer l’application de conversation, suivez les étapes dans le didacticiel [bien démarrer avec SignalR et MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Utilisez NuGet pour installer les bibliothèques requises. À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**. Dans le **Console du Gestionnaire de Package** fenêtre, entrez les commandes suivantes :

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Utilisez la `-ProjectName` option pour installer les packages du projet ASP.NET MVC, plutôt que le projet Windows Azure.

## <a name="configure-the-backplane"></a>Configurer le fond de panier

Dans le fichier Global.asax de votre application, ajoutez le code suivant :

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Vous devez maintenant obtenir votre chaîne de connexion de service bus. Dans le portail Azure, sélectionnez l’espace de noms service bus que vous avez créé et cliquez sur l’icône de clé d’accès.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Copier la chaîne de connexion dans le Presse-papiers, puis collez-la dans la *connectionString* variable.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Déployer sur Azure

Dans l’Explorateur de solutions, développez le **rôles** dossier dans le projet « ChatService ».

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Cliquez sur le rôle SignalRChat et sélectionnez **propriétés**. Sélectionnez le **Configuration** onglet. Sous **Instances** sélectionnez 2. Vous pouvez également définir la taille de machine virtuelle **très petite**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Enregistrez les modifications.

Dans l’Explorateur de solutions, cliquez sur le projet « ChatService ». Sélectionnez **Publier**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

S’il s’agit de votre première publication de temps sur Windows Azure, vous devez télécharger vos informations d’identification. Dans le **publier** Assistant, cliquez sur « Se connecter télécharger les informations d’identification ». Cela vous invitera à vous connecter au portail Windows Azure et télécharger un fichier de paramètres de publication.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Cliquez sur **importation** et sélectionnez le fichier de paramètres de publication que vous avez téléchargé.

Cliquez sur **Suivant**. Dans le **paramètres de publication** boîte de dialogue, sous **Service Cloud**, sélectionnez le service cloud que vous avez créé précédemment.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Cliquez sur **Publier**. Il peut prendre quelques minutes pour déployer l’application et de démarrer les machines virtuelles.

Maintenant lorsque vous exécutez l’application de conversation, les instances de rôle communiquent via Azure Service Bus, à l’aide d’une rubrique Service Bus. Une rubrique est une file d’attente qui permet à plusieurs abonnés.

Le fond de panier crée automatiquement la rubrique et les abonnements. Pour afficher les abonnements et l’activité des messages, ouvrez le portail Azure, sélectionnez l’espace de noms Service Bus et cliquez sur « Rubriques ».

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Il peut prendre quelques minutes avant que l’activité d’un message s’affiche dans le tableau de bord.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR gère la durée de vie de rubrique. Tant que votre application est déployée, n’essayez pas de supprimer les rubriques manuellement ou de modifier les paramètres sur le sujet.
