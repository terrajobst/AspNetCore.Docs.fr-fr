---
title: Le test de charge/contrainte ASP.NET Core
author: Jeremy-Meng
description: Découvrez plusieurs outils et approches de test de charge et de stress d’applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 0a8449ea2c9df0f2ac93058f03af0a1a2aa66508
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068181"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="ced7b-103">Le test de charge/contrainte ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ced7b-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="ced7b-104">Test de charge et tests de contrainte sont importantes pour garantir qu'une application web est performante et évolutive.</span><span class="sxs-lookup"><span data-stu-id="ced7b-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="ced7b-105">Leurs objectifs sont différents, même s’ils partagent souvent des tests similaires.</span><span class="sxs-lookup"><span data-stu-id="ced7b-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="ced7b-106">**Tests de charge** &ndash; tester si l’application peut gérer une charge spécifié d’utilisateurs pour un scénario certaines tout en répondant à l’objectif de réponse.</span><span class="sxs-lookup"><span data-stu-id="ced7b-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="ced7b-107">L’application est exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="ced7b-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="ced7b-108">**Tests de contrainte** &ndash; tester la stabilité d’application lors de l’exécution dans des conditions extrêmes, souvent pendant une longue période de temps.</span><span class="sxs-lookup"><span data-stu-id="ced7b-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="ced7b-109">Les tests de placer une charge utilisateur élevée, des pics ou charge croissante progressivement, sur l’application, ou elles limitent les ressources de calcul de l’application.</span><span class="sxs-lookup"><span data-stu-id="ced7b-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="ced7b-110">Tests de contrainte déterminent si une application en situation de stress peut récupérer et normalement revenir au comportement attendu.</span><span class="sxs-lookup"><span data-stu-id="ced7b-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="ced7b-111">En situation de stress, l’application n’est pas exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="ced7b-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="ced7b-112">Visual Studio 2019 est la dernière version de Visual Studio avec les fonctionnalités de test de charge.</span><span class="sxs-lookup"><span data-stu-id="ced7b-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="ced7b-113">Pour les clients nécessitant à l’avenir des outils de test de charge, nous vous recommandons d’autres outils, tels que Apache JMeter, Akamai CloudTest et BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="ced7b-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="ced7b-114">Pour plus d’informations, consultez le [Notes de publication de Visual Studio 2019](/visualstudio/releases/2019/release-notes#test-tools).</span><span class="sxs-lookup"><span data-stu-id="ced7b-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes#test-tools).</span></span>

<span data-ttu-id="ced7b-115">La tests de charge service dans Azure DevOps se termine dans 2020.</span><span class="sxs-lookup"><span data-stu-id="ced7b-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="ced7b-116">Pour plus d’informations, consultez [nuage un test de charge principal de service de durée de vie](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="ced7b-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="ced7b-117">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="ced7b-117">Visual Studio tools</span></span>

<span data-ttu-id="ced7b-118">Visual Studio permet aux utilisateurs de créer, développer et déboguer les tests de charge et de performances web.</span><span class="sxs-lookup"><span data-stu-id="ced7b-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="ced7b-119">Une option est disponible pour créer des tests à l’enregistrement des actions dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="ced7b-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="ced7b-120">Pour plus d’informations sur la façon de créer, configurer et exécuter un test de charge de projets à l’aide de Visual Studio 2017, consultez [Guide de démarrage rapide : créer un projet de test de charge](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="ced7b-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span> <span data-ttu-id="ced7b-121">Pour plus d’informations, consultez le [des ressources supplémentaires](#additional-resources) section.</span><span class="sxs-lookup"><span data-stu-id="ced7b-121">For more information, see the [Additional resources](#additional-resources) section.</span></span>

<span data-ttu-id="ced7b-122">Les tests de charge peuvent être configurés pour s’exécuter en local ou d’exécution dans le cloud à l’aide d’Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="ced7b-122">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="ced7b-123">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="ced7b-123">Azure DevOps</span></span>

<span data-ttu-id="ced7b-124">Les séries de tests de charge peuvent être démarrées à l’aide de la [Azure DevOps Plans de Test](/azure/devops/test/load-test/index?view=vsts) service.</span><span class="sxs-lookup"><span data-stu-id="ced7b-124">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Azure DevOps un test de charge page d’accueil](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="ced7b-126">Le service prend en charge les formats de test suivants :</span><span class="sxs-lookup"><span data-stu-id="ced7b-126">The service supports the following test formats:</span></span>

* <span data-ttu-id="ced7b-127">Visual Studio &ndash; test Web créé dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ced7b-127">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="ced7b-128">HTTP Archive &ndash; le trafic HTTP capturé à l’intérieur d’archive est relu pendant le test.</span><span class="sxs-lookup"><span data-stu-id="ced7b-128">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="ced7b-129">[Basé sur l’URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; permet de spécifier des URL pour charger le test, les types de demandes, les en-têtes et les chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="ced7b-129">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="ced7b-130">Exécuter la définition des paramètres tels que durée, modèle de charge et nombre d’utilisateurs peuvent être configurés.</span><span class="sxs-lookup"><span data-stu-id="ced7b-130">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="ced7b-131">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ced7b-131">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="ced7b-132">portail Azure</span><span class="sxs-lookup"><span data-stu-id="ced7b-132">Azure portal</span></span>

<span data-ttu-id="ced7b-133">[Le portail Azure permet la configuration et l’exécution du test de charge des applications web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directement à partir de la **performances** onglet d’App Service dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="ced7b-133">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure App Service dans le portail Azure](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="ced7b-135">Le test peut être un test manuel avec une URL spécifiée ou un fichier de Test Web Visual Studio, qui peut tester plusieurs URL.</span><span class="sxs-lookup"><span data-stu-id="ced7b-135">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Nouvelle page de Test de performances sur le portail Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="ced7b-137">À la fin du test, les rapports générés affichent les caractéristiques de performances de l’application.</span><span class="sxs-lookup"><span data-stu-id="ced7b-137">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="ced7b-138">Statistiques de l’exemple sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="ced7b-138">Example statistics include:</span></span>

* <span data-ttu-id="ced7b-139">Temps de réponse moyen</span><span class="sxs-lookup"><span data-stu-id="ced7b-139">Average response time</span></span>
* <span data-ttu-id="ced7b-140">Débit max. : demandes par seconde</span><span class="sxs-lookup"><span data-stu-id="ced7b-140">Max throughput: requests per second</span></span>
* <span data-ttu-id="ced7b-141">Pourcentage d’échec</span><span class="sxs-lookup"><span data-stu-id="ced7b-141">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="ced7b-142">Des outils tiers</span><span class="sxs-lookup"><span data-stu-id="ced7b-142">Third-party tools</span></span>

<span data-ttu-id="ced7b-143">La liste suivante contient des outils de performances web tierces avec différents ensembles de fonctionnalités :</span><span class="sxs-lookup"><span data-stu-id="ced7b-143">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="ced7b-144">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="ced7b-144">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="ced7b-145">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="ced7b-145">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="ced7b-146">Gatling</span><span class="sxs-lookup"><span data-stu-id="ced7b-146">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="ced7b-147">Climats</span><span class="sxs-lookup"><span data-stu-id="ced7b-147">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="ced7b-148">Ouest du vent WebSurge</span><span class="sxs-lookup"><span data-stu-id="ced7b-148">West Wind WebSurge</span></span>](http://websurge.west-wind.com/)
* [<span data-ttu-id="ced7b-149">Netling</span><span class="sxs-lookup"><span data-stu-id="ced7b-149">Netling</span></span>](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a><span data-ttu-id="ced7b-150">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ced7b-150">Additional resources</span></span>

* [<span data-ttu-id="ced7b-151">Série de blogs de Test de charge</span><span class="sxs-lookup"><span data-stu-id="ced7b-151">Load Test blog series</span></span>](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
