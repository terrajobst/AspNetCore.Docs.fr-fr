---
uid: mvc/overview/getting-started/introduction/getting-started
title: Bien démarrer avec ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici à l’aide de Visual Studio 2015. Le nouveau didacticiel utilise ASP.NET Core MVC 6, qui fournit de nombreuses improvem...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0585e3a841aef72a17d966041029ff7be129a2b3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402121"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="a3558-104">Bien démarrer avec ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="a3558-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="a3558-105">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a3558-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="a3558-106">Ce didacticiel vous apprend les notions de base de la création d’une application web ASP.NET MVC 5 avec [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a3558-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="a3558-107">Dernière Source pour le didacticiel situé sur [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="a3558-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="a3558-108">Ce didacticiel a été écrit par [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter : [ @shanselman ](https://twitter.com/shanselman) ) , et [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="a3558-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="a3558-109">Vous avez besoin d’un compte Azure pour déployer cette application dans Azure :</span><span class="sxs-lookup"><span data-stu-id="a3558-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="a3558-110">Vous pouvez [ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.</span><span class="sxs-lookup"><span data-stu-id="a3558-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="a3558-111">Vous pouvez [activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="a3558-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="a3558-112">Prise en main</span><span class="sxs-lookup"><span data-stu-id="a3558-112">Getting Started</span></span>

<span data-ttu-id="a3558-113">Démarrez en installant et en cours d’exécution [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a3558-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="a3558-114">Visual Studio est un environnement de développement intégré ou IDE.</span><span class="sxs-lookup"><span data-stu-id="a3558-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="a3558-115">Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications.</span><span class="sxs-lookup"><span data-stu-id="a3558-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="a3558-116">Dans Visual Studio, il existe une liste dans la partie inférieure affiche les différentes options disponibles pour vous.</span><span class="sxs-lookup"><span data-stu-id="a3558-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="a3558-117">Il existe également un menu qui fournit une autre façon d’effectuer des tâches dans l’IDE.</span><span class="sxs-lookup"><span data-stu-id="a3558-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="a3558-118">(Par exemple, au lieu de sélectionner **nouveau projet** à partir de la **Démarrer** page, vous pouvez utiliser le menu et sélectionnez **fichier** &gt; **denouveauprojet**.)</span><span class="sxs-lookup"><span data-stu-id="a3558-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="a3558-119">Créer votre première Application</span><span class="sxs-lookup"><span data-stu-id="a3558-119">Creating Your First Application</span></span>

<span data-ttu-id="a3558-120">Cliquez sur **nouveau projet**, puis sélectionnez Visual c# sur la gauche, puis **Web** , puis sélectionnez **Application Web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="a3558-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="a3558-121">Nommez votre projet « MvcMovie », puis **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3558-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="a3558-122">Dans le **nouveau projet ASP.NET** boîte de dialogue, cliquez sur **MVC** puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3558-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="a3558-123">Visual Studio a utilisé un modèle par défaut pour le projet ASP.NET MVC que vous venez de créer, afin que vous ayez une application opérationnelle sans rien faire dès maintenant !</span><span class="sxs-lookup"><span data-stu-id="a3558-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="a3558-124">Il s’agit d’un simple « Hello World ! »</span><span class="sxs-lookup"><span data-stu-id="a3558-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="a3558-125">projet et il l’un bon point de départ de votre application.</span><span class="sxs-lookup"><span data-stu-id="a3558-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="a3558-126">Cliquez sur F5 pour démarrer le débogage.</span><span class="sxs-lookup"><span data-stu-id="a3558-126">Click F5 to start debugging.</span></span> <span data-ttu-id="a3558-127">F5 provoque Visual Studio Démarrer [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) et exécuter votre application web.</span><span class="sxs-lookup"><span data-stu-id="a3558-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="a3558-128">Ensuite, Visual Studio lance un navigateur et ouvre la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3558-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="a3558-129">Notez que la barre d’adresses du navigateur indique `localhost:port#` et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a3558-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a3558-130">C’est parce que `localhost` pointe toujours vers votre ordinateur local, qui dans ce cas s’exécute l’application que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="a3558-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="a3558-131">Lorsque Visual Studio s’exécute à un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="a3558-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a3558-132">Dans l’image ci-dessous, le numéro de port est 1234.</span><span class="sxs-lookup"><span data-stu-id="a3558-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="a3558-133">Lorsque vous exécutez l’application, vous verrez un numéro de port différent.</span><span class="sxs-lookup"><span data-stu-id="a3558-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="a3558-134">Dès ce modèle par défaut vous donne les pages Accueil, Contact et sur.</span><span class="sxs-lookup"><span data-stu-id="a3558-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="a3558-135">L’image ci-dessus n’affiche pas le **accueil**, **sur** et **Contact** des liens.</span><span class="sxs-lookup"><span data-stu-id="a3558-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="a3558-136">Selon la taille de la fenêtre du navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher ces liens.</span><span class="sxs-lookup"><span data-stu-id="a3558-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="a3558-137">L’application fournit également la prise en charge pour vous inscrire et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="a3558-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="a3558-138">L’étape suivante consiste à modifier le fonctionnement de cette application et en savoir un peu sur ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a3558-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="a3558-139">Fermez l’application ASP.NET MVC et nous allons modifier du code.</span><span class="sxs-lookup"><span data-stu-id="a3558-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="a3558-140">Pour obtenir la liste de didacticiels actuels, consultez [MVC recommandé articles](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="a3558-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a3558-141">Consultez cette application en cours d’exécution sur Azure</span><span class="sxs-lookup"><span data-stu-id="a3558-141">See this App Running on Azure</span></span>

<span data-ttu-id="a3558-142">Vous souhaitez voir le site terminé en cours d’exécution en tant qu’une application web en direct ?</span><span class="sxs-lookup"><span data-stu-id="a3558-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a3558-143">Vous pouvez déployer une version complète de l’application à votre compte Azure en cliquant simplement sur le bouton suivant.</span><span class="sxs-lookup"><span data-stu-id="a3558-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="a3558-144">Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure.</span><span class="sxs-lookup"><span data-stu-id="a3558-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a3558-145">Si vous n’avez pas déjà un compte, vous disposez des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3558-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a3558-146">[Ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.</span><span class="sxs-lookup"><span data-stu-id="a3558-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a3558-147">[Activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.</span><span class="sxs-lookup"><span data-stu-id="a3558-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a3558-148">Next</span><span class="sxs-lookup"><span data-stu-id="a3558-148">Next</span></span>](adding-a-controller.md)
