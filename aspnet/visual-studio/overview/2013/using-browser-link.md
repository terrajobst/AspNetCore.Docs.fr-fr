---
uid: visual-studio/overview/2013/using-browser-link
title: À l’aide du lien du navigateur dans Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822193"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="8a83f-102">À l’aide du lien du navigateur dans Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8a83f-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="8a83f-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8a83f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8a83f-104">Lien du navigateur est une nouvelle fonctionnalité dans Visual Studio 2013 qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs web.</span><span class="sxs-lookup"><span data-stu-id="8a83f-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="8a83f-105">Vous pouvez utiliser le lien du navigateur pour actualiser votre application web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests dans plusieurs navigateurs.</span><span class="sxs-lookup"><span data-stu-id="8a83f-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="8a83f-106">Actualisation du navigateur</span><span class="sxs-lookup"><span data-stu-id="8a83f-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="8a83f-107">Affichage du tableau de bord de lien de navigateur</span><span class="sxs-lookup"><span data-stu-id="8a83f-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="8a83f-108">Activation du lien de navigateur pour les fichiers HTML statiques</span><span class="sxs-lookup"><span data-stu-id="8a83f-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="8a83f-109">La désactivation de lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="8a83f-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="8a83f-110">Comment cela fonctionne-t-il ?</span><span class="sxs-lookup"><span data-stu-id="8a83f-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="8a83f-111">Actualisation du navigateur</span><span class="sxs-lookup"><span data-stu-id="8a83f-111">Browser Refresh</span></span>

<span data-ttu-id="8a83f-112">Avec l’actualisation du navigateur, vous pouvez actualiser plusieurs navigateurs qui sont connectés à Visual Studio via le lien du navigateur.</span><span class="sxs-lookup"><span data-stu-id="8a83f-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="8a83f-113">Pour utiliser l’actualisation du navigateur, d’abord créer une application ASP.NET, à l’aide d’un des modèles de projet.</span><span class="sxs-lookup"><span data-stu-id="8a83f-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="8a83f-114">Déboguer l’application en appuyant sur F5 ou en cliquant sur l’icône de flèche dans la barre d’outils :</span><span class="sxs-lookup"><span data-stu-id="8a83f-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="8a83f-115">Vous pouvez également utiliser la liste déroulante pour sélectionner un navigateur spécifique pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="8a83f-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="8a83f-116">Pour déboguer avec plusieurs navigateurs, sélectionnez **naviguer avec**.</span><span class="sxs-lookup"><span data-stu-id="8a83f-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="8a83f-117">Dans le **naviguer avec** boîte de dialogue, maintenez enfoncée la touche CTRL enfoncée pour sélectionner plusieurs navigateurs.</span><span class="sxs-lookup"><span data-stu-id="8a83f-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="8a83f-118">Cliquez sur **Parcourir** pour déboguer avec les navigateurs sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="8a83f-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="8a83f-119">Lien du navigateur fonctionne également si vous lancez un navigateur en dehors de Visual Studio et accédez à l’URL de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a83f-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="8a83f-120">Les contrôles de lien du navigateur se trouvent dans la liste déroulante avec l’icône de flèche circulaire.</span><span class="sxs-lookup"><span data-stu-id="8a83f-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="8a83f-121">L’icône de flèche est la **Actualiser** bouton.</span><span class="sxs-lookup"><span data-stu-id="8a83f-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="8a83f-122">Pour voir quels sont les navigateurs sont connectés, pointez la souris sur le **Actualiser** bouton pendant le débogage.</span><span class="sxs-lookup"><span data-stu-id="8a83f-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="8a83f-123">Les navigateurs connectés sont affichés dans une fenêtre d’info-bulle.</span><span class="sxs-lookup"><span data-stu-id="8a83f-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="8a83f-124">Pour actualiser les navigateurs connectés, cliquez sur le **Actualiser** bouton ou appuyez sur CTRL + ALT + ENTRÉE.</span><span class="sxs-lookup"><span data-stu-id="8a83f-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="8a83f-125">Par exemple, la capture d’écran suivante montre un projet ASP.NET, que j’ai créé à l’aide du modèle de projet MVC 5.</span><span class="sxs-lookup"><span data-stu-id="8a83f-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="8a83f-126">Vous pouvez voir l’application en cours d’exécution dans les deux navigateurs en haut.</span><span class="sxs-lookup"><span data-stu-id="8a83f-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="8a83f-127">En bas, le projet est ouvert dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a83f-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="8a83f-128">Dans Visual Studio, j’ai modifié le &lt;h1&gt; titre pour la page d’accueil :</span><span class="sxs-lookup"><span data-stu-id="8a83f-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="8a83f-129">Lorsque j’ai cliqué sur le **Actualiser** bouton, la modification est apparue dans les deux fenêtres du navigateur :</span><span class="sxs-lookup"><span data-stu-id="8a83f-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="8a83f-130">**Notes**</span><span class="sxs-lookup"><span data-stu-id="8a83f-130">**Notes**</span></span>

- <span data-ttu-id="8a83f-131">Pour activer le lien du navigateur, définissez `debug=true` dans le [ &lt;compilation&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) élément dans le fichier Web.config du projet.</span><span class="sxs-lookup"><span data-stu-id="8a83f-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="8a83f-132">L’application doit être en cours d’exécution sur l’hôte local.</span><span class="sxs-lookup"><span data-stu-id="8a83f-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="8a83f-133">L’application doit cibler .NET 4.0 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="8a83f-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="8a83f-134">Affichage du tableau de bord de lien de navigateur</span><span class="sxs-lookup"><span data-stu-id="8a83f-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="8a83f-135">Le tableau de bord lien du navigateur affiche des informations sur les connexions de lien du navigateur.</span><span class="sxs-lookup"><span data-stu-id="8a83f-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="8a83f-136">Pour afficher le tableau de bord, sélectionnez le menu déroulant de lien du navigateur (la petite flèche à côté du **Actualiser** bouton).</span><span class="sxs-lookup"><span data-stu-id="8a83f-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="8a83f-137">Puis cliquez sur **tableau de bord de lien de navigateur**.</span><span class="sxs-lookup"><span data-stu-id="8a83f-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="8a83f-138">Le tableau de bord répertorie les navigateurs connectés et l’URL à laquelle chaque navigateur a navigué.</span><span class="sxs-lookup"><span data-stu-id="8a83f-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="8a83f-139">Le **conditions préalables** section montre toutes les étapes nécessaires pour activer le lien du navigateur pour ce projet.</span><span class="sxs-lookup"><span data-stu-id="8a83f-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="8a83f-140">Par exemple, la capture d’écran suivante montre un projet où « debug » a la valeur false dans le fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="8a83f-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="8a83f-141">Activation du lien de navigateur pour les fichiers HTML statiques</span><span class="sxs-lookup"><span data-stu-id="8a83f-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="8a83f-142">Pour activer le lien du navigateur pour les fichiers HTML statiques, ajoutez le code suivant à votre fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="8a83f-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="8a83f-143">Pour des raisons de performances, supprimez ce paramètre lorsque vous publiez votre projet.</span><span class="sxs-lookup"><span data-stu-id="8a83f-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="8a83f-144">La désactivation de lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="8a83f-144">Disabling Browser Link</span></span>

<span data-ttu-id="8a83f-145">Lien du navigateur est activé par défaut.</span><span class="sxs-lookup"><span data-stu-id="8a83f-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="8a83f-146">Il existe plusieurs façons de le désactiver :</span><span class="sxs-lookup"><span data-stu-id="8a83f-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="8a83f-147">Dans le menu déroulant du lien du navigateur, décochez la case **activer le lien du navigateur**.</span><span class="sxs-lookup"><span data-stu-id="8a83f-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="8a83f-148">Dans le fichier Web.config, ajoutez une clé nommée « vs : EnableBrowserLink » avec la valeur « false » dans la section appSettings.</span><span class="sxs-lookup"><span data-stu-id="8a83f-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="8a83f-149">Dans le fichier Web.config, définissez le débogage sur false.</span><span class="sxs-lookup"><span data-stu-id="8a83f-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="8a83f-150">Comment cela fonctionne-t-il ?</span><span class="sxs-lookup"><span data-stu-id="8a83f-150">How Does It Work?</span></span>

<span data-ttu-id="8a83f-151">Lien du navigateur utilise [SignalR](../../../signalr/index.md) pour créer un canal de communication entre Visual Studio et le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8a83f-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="8a83f-152">Quand le lien du navigateur est activé, Visual Studio fait office de serveur SignalR auquel plusieurs clients (navigateurs) peuvent se connecter. </span><span class="sxs-lookup"><span data-stu-id="8a83f-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="8a83f-153">Lien du navigateur enregistre également un module HTTP avec ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8a83f-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="8a83f-154">Ce module injecte spéciale &lt;script&gt; références dans chaque demande de page à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="8a83f-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="8a83f-155">Vous pouvez voir les références de script en sélectionnant « Afficher la source » dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8a83f-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="8a83f-156">Vos fichiers sources ne sont pas modifiées.</span><span class="sxs-lookup"><span data-stu-id="8a83f-156">Your source files are not modified.</span></span> <span data-ttu-id="8a83f-157">Le module HTTP injecte dynamiquement les références de script.</span><span class="sxs-lookup"><span data-stu-id="8a83f-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="8a83f-158">Le code côté navigateur étant toutes les fonctions JavaScript, il fonctionne sur tous les navigateurs qui [SignalR prend en charge](../../../signalr/overview/getting-started/supported-platforms.md), sans nécessiter de n’importe quel plug-in de navigateur.</span><span class="sxs-lookup"><span data-stu-id="8a83f-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
