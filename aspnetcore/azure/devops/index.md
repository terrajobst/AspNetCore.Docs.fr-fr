---
title: DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Un guide qui fournit des conseils de bout en bout sur la création d’un pipeline DevOps pour une application ASP.NET Core hébergée dans Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722530"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="fb30d-103">DevOps avec ASP.NET Core et Azure</span><span class="sxs-lookup"><span data-stu-id="fb30d-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="fb30d-104">Bienvenue dans le guide du cycle de vie du développement Azure pour .NET !</span><span class="sxs-lookup"><span data-stu-id="fb30d-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="fb30d-105">Ce guide présente les concepts de base de la création d’un cycle de vie du développement autour d’Azure avec les outils et les processus .NET.</span><span class="sxs-lookup"><span data-stu-id="fb30d-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="fb30d-106">Après avoir terminé ce guide, vous pourrez tirer parti d’une chaîne d’outils DevOps arrivés à maturation.</span><span class="sxs-lookup"><span data-stu-id="fb30d-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="fb30d-107">Audience à laquelle ce guide est destiné</span><span class="sxs-lookup"><span data-stu-id="fb30d-107">Who this guide is for</span></span>

<span data-ttu-id="fb30d-108">Vous devez être un développeur ASP.NET expérimenté (niveau 200-300).</span><span class="sxs-lookup"><span data-stu-id="fb30d-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="fb30d-109">Vous n’avez pas besoin de connaissances sur Azure, car nous couvrons cet aspect dans cette introduction.</span><span class="sxs-lookup"><span data-stu-id="fb30d-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="fb30d-110">Ce guide peut également être utile aux ingénieurs DevOps qui se consacrent davantage aux opérations qu’au développement.</span><span class="sxs-lookup"><span data-stu-id="fb30d-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="fb30d-111">Ce guide cible les développeurs Windows.</span><span class="sxs-lookup"><span data-stu-id="fb30d-111">This guide targets Windows developers.</span></span> <span data-ttu-id="fb30d-112">Cependant, Linux et macOS sont entièrement pris en charge par .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb30d-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="fb30d-113">Pour adapter ce guide à Linux/macOS, consultez les indications faisant état des différences pour Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="fb30d-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="fb30d-114">Ce que ce guide ne couvre pas</span><span class="sxs-lookup"><span data-stu-id="fb30d-114">What this guide doesn't cover</span></span>

<span data-ttu-id="fb30d-115">Ce guide est centré sur une expérience de déploiement continu de bout en bout pour les développeurs .NET.</span><span class="sxs-lookup"><span data-stu-id="fb30d-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="fb30d-116">Il ne s’agit pas d’un guide exhaustif sur Azure, et il n’aborde pas de façon détaillée les API .NET pour les services Azure.</span><span class="sxs-lookup"><span data-stu-id="fb30d-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="fb30d-117">L’accent est mis sur tout ce qui a trait à l’intégration continue, au déploiement, à la surveillance et au débogage.</span><span class="sxs-lookup"><span data-stu-id="fb30d-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="fb30d-118">Vous pouvez trouver des recommandations pour les étapes suivantes vers la fin du guide.</span><span class="sxs-lookup"><span data-stu-id="fb30d-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="fb30d-119">Les services de plateforme Azure qui sont utiles aux développeurs ASP.NET figurent dans les suggestions.</span><span class="sxs-lookup"><span data-stu-id="fb30d-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="fb30d-120">Contenu de ce guide</span><span class="sxs-lookup"><span data-stu-id="fb30d-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="fb30d-121">Outils et téléchargements</span><span class="sxs-lookup"><span data-stu-id="fb30d-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="fb30d-122">Découvrez où vous procurer les outils utilisés dans ce guide.</span><span class="sxs-lookup"><span data-stu-id="fb30d-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="fb30d-123">Déployer sur App Service</span><span class="sxs-lookup"><span data-stu-id="fb30d-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="fb30d-124">Découvrez les différentes méthodes de déploiement d’une application ASP.NET Core sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="fb30d-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="fb30d-125">Intégration et déploiement continus</span><span class="sxs-lookup"><span data-stu-id="fb30d-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="fb30d-126">Créez une solution d’intégration et de déploiement continus de bout en bout pour votre application ASP.NET Core avec GitHub, VSTS et Azure.</span><span class="sxs-lookup"><span data-stu-id="fb30d-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="fb30d-127">Surveiller et déboguer</span><span class="sxs-lookup"><span data-stu-id="fb30d-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="fb30d-128">Utilisez les outils d’Azure pour surveiller, dépanner et optimiser votre application.</span><span class="sxs-lookup"><span data-stu-id="fb30d-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="fb30d-129">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fb30d-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="fb30d-130">Autres parcours d’apprentissage pour les développeurs ASP.NET Core découvrant Azure.</span><span class="sxs-lookup"><span data-stu-id="fb30d-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="fb30d-131">Remerciements</span><span class="sxs-lookup"><span data-stu-id="fb30d-131">Acknowledgments</span></span>

<span data-ttu-id="fb30d-132">Nous remercions tous les membres de la communauté .NET qui ont contribué à ce guide pour leurs suggestions pertinentes !</span><span class="sxs-lookup"><span data-stu-id="fb30d-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="fb30d-133">Nous aimerions en particulier remercier les membres suivants de la communauté, qui ont contribué à la révision finale de ce document :</span><span class="sxs-lookup"><span data-stu-id="fb30d-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="fb30d-134">Sam Wronski</span><span class="sxs-lookup"><span data-stu-id="fb30d-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="fb30d-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="fb30d-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="fb30d-136">Conclusion</span><span class="sxs-lookup"><span data-stu-id="fb30d-136">Conclusion</span></span>

<span data-ttu-id="fb30d-137">Ce guide vous prépare à créer un cycle de vie de développement d’intégration continue construit autour d’ASP.NET Core et d’Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="fb30d-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="fb30d-138">Lecture supplémentaire</span><span class="sxs-lookup"><span data-stu-id="fb30d-138">Additional reading</span></span>

* [<span data-ttu-id="fb30d-139">Qu’est-ce que le cloud computing ?</span><span class="sxs-lookup"><span data-stu-id="fb30d-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="fb30d-140">Exemples de cloud computing</span><span class="sxs-lookup"><span data-stu-id="fb30d-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="fb30d-141">Qu’est-ce que IaaS ?</span><span class="sxs-lookup"><span data-stu-id="fb30d-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="fb30d-142">Qu’est-ce que PaaS ?</span><span class="sxs-lookup"><span data-stu-id="fb30d-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
