---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: La mise à niveau des projets de 1.x SignalR vers la version 2 | Documents Microsoft
author: pfletcher
description: Cette rubrique décrit comment mettre à niveau un projet de 1.x SignalR existant pour SignalR 2.x et comment résoudre les problèmes qui peuvent survenir pendant le processus de mise à niveau...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505738"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="44deb-103">La mise à niveau des projets de 1.x SignalR vers la version 2</span><span class="sxs-lookup"><span data-stu-id="44deb-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="44deb-104">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="44deb-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="44deb-105">Cette rubrique décrit comment mettre à niveau un projet de 1.x SignalR existant pour SignalR 2.x et comment résoudre les problèmes qui peuvent survenir pendant le processus de mise à niveau.</span><span class="sxs-lookup"><span data-stu-id="44deb-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="44deb-106">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="44deb-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="44deb-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="44deb-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="44deb-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="44deb-108">.NET 4.5</span></span>
> - <span data-ttu-id="44deb-109">SignalR versions 1 et 2</span><span class="sxs-lookup"><span data-stu-id="44deb-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="44deb-110">À l’aide de Visual Studio 2012 avec ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="44deb-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="44deb-111">Pour utiliser Visual Studio 2012 avec ce didacticiel, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="44deb-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="44deb-112">Mise à jour votre [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="44deb-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="44deb-113">Installer le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="44deb-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="44deb-114">Dans Web Platform Installer, recherchez et installez **ASP.NET et 2013.1 des outils Web pour Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="44deb-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="44deb-115">Il installe les modèles Visual Studio pour les classes de SignalR comme **Hub**.</span><span class="sxs-lookup"><span data-stu-id="44deb-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="44deb-116">Certains modèles (tel que **classe de démarrage OWIN**) ne sera pas disponible ; dans ce cas, utilisez un fichier de classe à la place.</span><span class="sxs-lookup"><span data-stu-id="44deb-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="44deb-117">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="44deb-117">Questions and comments</span></span>
> 
> <span data-ttu-id="44deb-118">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="44deb-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="44deb-119">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="44deb-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="44deb-120">SignalR 2 offre une expérience de développement cohérent entre les plateformes de serveur à l’aide de [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="44deb-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="44deb-121">Cet article décrit les étapes nécessaires pour mettre à jour d’une application de 1.x SignalR vers la version 2.</span><span class="sxs-lookup"><span data-stu-id="44deb-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="44deb-122">Il est recommandé de mettre à niveau des applications à 2 de SignalR, SignalR 1.x sont toujours être prise en charge.</span><span class="sxs-lookup"><span data-stu-id="44deb-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="44deb-123">Ce didacticiel explique comment mettre à niveau une application web hébergée à SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="44deb-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="44deb-124">Les applications auto-hébergées (ceux qui hébergent un serveur dans une application console, le service Windows ou autre processus) sont désormais pris en charge sous SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="44deb-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="44deb-125">Pour plus d’informations sur la façon de commencer à créer une application auto-hébergée avec SignalR 2, consultez [didacticiel : SignalR auto-hébergement](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="44deb-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="44deb-126">Sommaire</span><span class="sxs-lookup"><span data-stu-id="44deb-126">Contents</span></span>

<span data-ttu-id="44deb-127">Les sections suivantes décrivent les tâches impliquées dans la mise à niveau des projets de SignalR et comment résoudre les problèmes qui peuvent survenir.</span><span class="sxs-lookup"><span data-stu-id="44deb-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="44deb-128">Exemple : La mise à niveau le didacticiel mise en route pour SignalR 2</span><span class="sxs-lookup"><span data-stu-id="44deb-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="44deb-129">Dépannage des erreurs rencontrées lors de la mise à niveau</span><span class="sxs-lookup"><span data-stu-id="44deb-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="44deb-130">Exemple : La mise à niveau de l’application du didacticiel mise en route pour SignalR 2</span><span class="sxs-lookup"><span data-stu-id="44deb-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="44deb-131">Dans cette section, vous allez mettre à jour l’application créée dans le [version 1.x de SignalR du didacticiel de mise en route](../older-versions/index.md) pour utiliser 2 de SignalR.</span><span class="sxs-lookup"><span data-stu-id="44deb-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="44deb-132">Une fois que vous avez terminé le didacticiel de mise en route, avec le bouton droit sur le projet, puis sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="44deb-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="44deb-133">Vérifiez que le **framework cible** a la valeur **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="44deb-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="44deb-134">Ouvrez la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="44deb-134">Open the Package Manager Console.</span></span> <span data-ttu-id="44deb-135">Supprimer SignalR 1.x à partir du projet à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="44deb-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="44deb-136">Installer 2 SignalR à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="44deb-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="44deb-137">Dans la page HTML, mettre à jour la référence de script pour SignalR correspondre à la version du script maintenant inclus dans le projet.</span><span class="sxs-lookup"><span data-stu-id="44deb-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="44deb-138">Dans la classe d’application globale, supprimez l’appel à MapHubs.</span><span class="sxs-lookup"><span data-stu-id="44deb-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="44deb-139">Avec le bouton droit de la solution, puis sélectionnez **ajouter**, **un nouvel élément...** . Dans la boîte de dialogue, sélectionnez **classe de démarrage Owin**.</span><span class="sxs-lookup"><span data-stu-id="44deb-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="44deb-140">Nommez la nouvelle classe **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="44deb-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="44deb-141">Remplacez le contenu de Startup.cs par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="44deb-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="44deb-142">L’attribut d’assembly ajoute la classe au processus de démarrage de Owin, qui exécute le `Configuration` méthode au démarrage de Owin.</span><span class="sxs-lookup"><span data-stu-id="44deb-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="44deb-143">Cette action appelle à son tour le `MapSignalR` (méthode), ce qui crée des itinéraires pour tous les concentrateurs SignalR dans l’application.</span><span class="sxs-lookup"><span data-stu-id="44deb-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="44deb-144">Exécutez le projet et copiez l’URL de la page principale dans un autre navigateur ou le volet du navigateur, comme précédemment.</span><span class="sxs-lookup"><span data-stu-id="44deb-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="44deb-145">Chaque page vous invite à entrer un nom d’utilisateur et les messages envoyés à partir de chaque page doivent être visibles dans les deux volets de navigateur.</span><span class="sxs-lookup"><span data-stu-id="44deb-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="44deb-146">Dépannage des erreurs rencontrées lors de la mise à niveau</span><span class="sxs-lookup"><span data-stu-id="44deb-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="44deb-147">Cette section décrit les problèmes qui peuvent survenir lors de la mise à niveau.</span><span class="sxs-lookup"><span data-stu-id="44deb-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="44deb-148">Pour obtenir une liste plus complète des erreurs et les problèmes qui peuvent se produire avec une application SignalR, consultez [SignalR dépannage](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="44deb-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="44deb-149">'L’appel est ambigu entre les méthodes ou propriétés suivantes'</span><span class="sxs-lookup"><span data-stu-id="44deb-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="44deb-150">Cette erreur se produit si une référence à `Microsoft.AspNet.SignalR.Owin` n’est pas supprimé.</span><span class="sxs-lookup"><span data-stu-id="44deb-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="44deb-151">Ce package est déconseillé ; la référence doit être supprimée, et la version 1.x du package SelfHost doit être désinstallée.</span><span class="sxs-lookup"><span data-stu-id="44deb-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="44deb-152">Méthodes de concentrateur échouent en silence</span><span class="sxs-lookup"><span data-stu-id="44deb-152">Hub methods fail silently</span></span>

<span data-ttu-id="44deb-153">Vérifiez que les références de script dans votre client sont à jour, ainsi que le `OwinStartup` d’attributs pour votre classe de démarrage a la classe appropriée et noms d’assembly pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="44deb-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="44deb-154">En outre, essayez d’ouvrir l’adresse hubs (concentrateurs signalr / /) dans votre navigateur. toute erreur qui s’affiche propose de plus d’informations sur ce qui se passe incorrect.</span><span class="sxs-lookup"><span data-stu-id="44deb-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
