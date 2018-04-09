---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publier l’application dans Azure App Service de Azure | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="ece55-102">Publier l’application dans Azure App Service de Azure</span><span class="sxs-lookup"><span data-stu-id="ece55-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="ece55-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ece55-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ece55-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="ece55-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ece55-105">La dernière étape, vous allez publier l’application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="ece55-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="ece55-106">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="ece55-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="ece55-107">En cliquant sur **publier** appelle la **publier le site Web** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="ece55-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="ece55-108">Si vous avez coché **hôte du Cloud** lorsque vous avez créé le projet, puis la connexion et les paramètres sont déjà configurés.</span><span class="sxs-lookup"><span data-stu-id="ece55-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="ece55-109">Dans ce cas, cliquez simplement sur le **paramètres** et vérifiez &quot;exécuter des Migrations Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ece55-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ece55-110">(Si vous n’avez pas vérifier **hôte du Cloud** au début, puis suivez les étapes de la [section suivante](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="ece55-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="ece55-111">Pour déployer l’application, cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="ece55-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="ece55-112">Vous pouvez afficher la progression de la publication dans le **de l’activité de publication Web** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="ece55-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="ece55-113">(À partir de la **vue** menu, sélectionnez **autres fenêtres**, puis sélectionnez **de l’activité de publication Web**.)</span><span class="sxs-lookup"><span data-stu-id="ece55-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="ece55-114">Visual Studio après déploiement de l’application, le navigateur par défaut s’ouvre automatiquement à l’URL du site Web déployé, et l’application que vous avez créé est en cours d’exécution dans le cloud.</span><span class="sxs-lookup"><span data-stu-id="ece55-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="ece55-115">L’URL dans la barre d’adresse de navigateur montre que le site est chargé à partir d’Internet.</span><span class="sxs-lookup"><span data-stu-id="ece55-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="ece55-116">Déploiement vers un nouveau site Web</span><span class="sxs-lookup"><span data-stu-id="ece55-116">Deploying to a New Website</span></span>

<span data-ttu-id="ece55-117">Si vous n’avez pas coché **hôte du Cloud** lors de la création du projet, vous pouvez désormais configurer une application web.</span><span class="sxs-lookup"><span data-stu-id="ece55-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="ece55-118">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="ece55-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="ece55-119">Sélectionnez le **profil** onglet et cliquez sur **Microsoft Azure Websites**.</span><span class="sxs-lookup"><span data-stu-id="ece55-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="ece55-120">Si vous n’êtes pas actuellement connecté à Azure, vous devrez vous connecter.</span><span class="sxs-lookup"><span data-stu-id="ece55-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="ece55-121">Dans le **sites Web existants** boîte de dialogue, cliquez sur **nouveau**.</span><span class="sxs-lookup"><span data-stu-id="ece55-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="ece55-122">Entrez un nom de site.</span><span class="sxs-lookup"><span data-stu-id="ece55-122">Enter a site name.</span></span> <span data-ttu-id="ece55-123">Sélectionnez votre abonnement Azure et la région.</span><span class="sxs-lookup"><span data-stu-id="ece55-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="ece55-124">Sous **serveur de base de données**, sélectionnez **créer un nouveau serveur**, ou sélectionnez un serveur existant.</span><span class="sxs-lookup"><span data-stu-id="ece55-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="ece55-125">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ece55-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="ece55-126">Cliquez sur le **paramètres** et vérifiez &quot;exécuter des Migrations Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="ece55-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="ece55-127">Puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="ece55-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ece55-128">Précédent</span><span class="sxs-lookup"><span data-stu-id="ece55-128">Previous</span></span>](part-9.md)
