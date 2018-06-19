---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utiliser OWIN pour l’auto-hébergement ASP.NET Web API 2 | Documents Microsoft
author: rick-anderson
description: Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide de OWIN pour l’auto-hébergement l’infrastructure API Web. Ouvrez l’Interface Web pour .NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506968"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="38377-104">Utiliser OWIN pour l’auto-hébergement API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="38377-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="38377-105">par [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="38377-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="38377-106">Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide de OWIN pour l’auto-hébergement l’infrastructure API Web.</span><span class="sxs-lookup"><span data-stu-id="38377-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="38377-107">[Ouvrir l’Interface Web pour .NET](http://owin.org) (OWIN) définit une abstraction entre les serveurs web .NET et des applications web.</span><span class="sxs-lookup"><span data-stu-id="38377-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="38377-108">OWIN dissocie l’application web à partir du serveur, ce qui rend OWIN idéale pour l’auto-hébergement une application web dans votre propre processus, en dehors d’IIS.</span><span class="sxs-lookup"><span data-stu-id="38377-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="38377-109">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="38377-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="38377-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (fonctionne également avec Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="38377-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="38377-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="38377-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="38377-112">Vous pouvez trouver le code source complet pour ce didacticiel à [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="38377-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="38377-113">Créez une Application Console</span><span class="sxs-lookup"><span data-stu-id="38377-113">Create a Console Application</span></span>

<span data-ttu-id="38377-114">Sur le **fichier** menu, cliquez sur **nouveau**, puis cliquez sur **projet**.</span><span class="sxs-lookup"><span data-stu-id="38377-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="38377-115">À partir de **modèles installés**, sous Visual c#, cliquez sur **Windows** puis cliquez sur **Application Console**.</span><span class="sxs-lookup"><span data-stu-id="38377-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="38377-116">Nommez le projet « OwinSelfhostSample » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="38377-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="38377-117">Ajouter les API Web et les Packages OWIN</span><span class="sxs-lookup"><span data-stu-id="38377-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="38377-118">À partir de la **outils** menu, cliquez sur **Gestionnaire de Package de bibliothèque**, puis cliquez sur **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="38377-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="38377-119">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="38377-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="38377-120">Cela installe le package selfhost WebAPI OWIN et tous les packages OWIN requis.</span><span class="sxs-lookup"><span data-stu-id="38377-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="38377-121">Configurer les API Web pour auto-hébergement</span><span class="sxs-lookup"><span data-stu-id="38377-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="38377-122">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="38377-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="38377-123">Nommez la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="38377-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="38377-124">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="38377-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="38377-125">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="38377-125">Add a Web API Controller</span></span>

<span data-ttu-id="38377-126">Ensuite, ajoutez une classe de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="38377-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="38377-127">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="38377-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="38377-128">Nommez la classe `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="38377-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="38377-129">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="38377-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="38377-130">Démarrer l’hôte OWIN et effectuer une demande à l’aide du client HTTP</span><span class="sxs-lookup"><span data-stu-id="38377-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="38377-131">Remplacez tout le code réutilisable dans le fichier Program.cs par le suivant :</span><span class="sxs-lookup"><span data-stu-id="38377-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="38377-132">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="38377-132">Running the Application</span></span>

<span data-ttu-id="38377-133">Pour exécuter l’application, appuyez sur F5 dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="38377-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="38377-134">La sortie doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="38377-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="38377-135">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="38377-135">Additional Resources</span></span>

[<span data-ttu-id="38377-136">Une vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="38377-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="38377-137">Héberger des API Web ASP.NET dans un rôle de travail Azure</span><span class="sxs-lookup"><span data-stu-id="38377-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
