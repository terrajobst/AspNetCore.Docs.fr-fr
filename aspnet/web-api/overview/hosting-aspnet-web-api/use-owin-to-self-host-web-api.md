---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utiliser OWIN pour auto-héberger l’API Web ASP.NET | Microsoft Docs
author: rick-anderson
description: Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web. Open Web Interface pour .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667386"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="f8a02-104">Utiliser OWIN pour auto-héberger l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f8a02-104">Use OWIN to Self-Host ASP.NET Web API</span></span> 
====================

> <span data-ttu-id="f8a02-105">Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web.</span><span class="sxs-lookup"><span data-stu-id="f8a02-105">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="f8a02-106">[Open Web Interface pour .NET](http://owin.org) (OWIN) définit une abstraction entre les serveurs web de .NET et des applications web.</span><span class="sxs-lookup"><span data-stu-id="f8a02-106">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f8a02-107">OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS.</span><span class="sxs-lookup"><span data-stu-id="f8a02-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f8a02-108">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="f8a02-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="f8a02-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f8a02-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="f8a02-110">API 5.2.7 Web</span><span class="sxs-lookup"><span data-stu-id="f8a02-110">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="f8a02-111">Vous trouverez le code source complet pour ce didacticiel à [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="f8a02-111">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="f8a02-112">Créer une application console</span><span class="sxs-lookup"><span data-stu-id="f8a02-112">Create a console application</span></span>

<span data-ttu-id="f8a02-113">Sur le **fichier** menu, **New**, puis sélectionnez **projet**.</span><span class="sxs-lookup"><span data-stu-id="f8a02-113">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="f8a02-114">À partir de **installé**, sous **Visual C#** , sélectionnez **Windows Desktop** , puis sélectionnez **application Console (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="f8a02-114">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="f8a02-115">Nommez le projet « OwinSelfhostSample » et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8a02-115">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="f8a02-116">Ajoutez les packages de l’API Web et la bibliothèque OWIN</span><span class="sxs-lookup"><span data-stu-id="f8a02-116">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="f8a02-117">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="f8a02-117">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f8a02-118">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f8a02-118">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="f8a02-119">Cela installera le package de selfhost WebAPI OWIN et tous les packages OWIN requis.</span><span class="sxs-lookup"><span data-stu-id="f8a02-119">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="f8a02-120">Configurer les API Web pour Self-host</span><span class="sxs-lookup"><span data-stu-id="f8a02-120">Configure Web API for self-host</span></span>

<span data-ttu-id="f8a02-121">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="f8a02-121">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f8a02-122">Nommez la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f8a02-122">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="f8a02-123">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f8a02-123">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="f8a02-124">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="f8a02-124">Add a Web API controller</span></span>

<span data-ttu-id="f8a02-125">Ensuite, ajoutez une classe de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="f8a02-125">Next, add a Web API controller class.</span></span> <span data-ttu-id="f8a02-126">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="f8a02-126">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f8a02-127">Nommez la classe `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="f8a02-127">Name the class `ValuesController`.</span></span>

<span data-ttu-id="f8a02-128">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f8a02-128">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="f8a02-129">Démarrer l’hôte OWIN et faire une demande avec HttpClient</span><span class="sxs-lookup"><span data-stu-id="f8a02-129">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="f8a02-130">Remplacez tout le code réutilisable dans le fichier Program.cs par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f8a02-130">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="f8a02-131">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="f8a02-131">Run the application</span></span>

<span data-ttu-id="f8a02-132">Pour exécuter l’application, appuyez sur F5 dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8a02-132">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="f8a02-133">La sortie doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="f8a02-133">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="f8a02-134">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f8a02-134">Additional resources</span></span>

[<span data-ttu-id="f8a02-135">Vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="f8a02-135">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="f8a02-136">Héberger des API Web ASP.NET dans un rôle Worker Azure</span><span class="sxs-lookup"><span data-stu-id="f8a02-136">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
