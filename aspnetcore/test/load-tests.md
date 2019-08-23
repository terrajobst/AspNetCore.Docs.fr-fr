---
title: ASP.NET Core le test de charge/stress
author: Jeremy-Meng
description: Découvrez plusieurs outils et approches notables pour le test de charge et les tests de stress ASP.NET Core les applications.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975247"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="5e54f-103">ASP.NET Core le test de charge/stress</span><span class="sxs-lookup"><span data-stu-id="5e54f-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="5e54f-104">Les tests de charge et les tests de stress sont importants pour garantir qu’une application Web est performante et évolutive.</span><span class="sxs-lookup"><span data-stu-id="5e54f-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="5e54f-105">Leurs objectifs sont différents même s’ils partagent souvent des tests similaires.</span><span class="sxs-lookup"><span data-stu-id="5e54f-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="5e54f-106">**Tests de charge** &ndash; Teste si l’application peut gérer une charge d’utilisateurs spécifiée pour un certain scénario tout en répondant à l’objectif de réponse.</span><span class="sxs-lookup"><span data-stu-id="5e54f-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="5e54f-107">L’application est exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="5e54f-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="5e54f-108">**Tests de stress** &ndash; Testez la stabilité des applications en cas d’exécution dans des conditions extrêmes, souvent pendant une longue période de temps.</span><span class="sxs-lookup"><span data-stu-id="5e54f-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="5e54f-109">Les tests mettent en place une charge utilisateur élevée, des pics ou une augmentation progressive de la charge, sur l’application, ou ils limitent les ressources informatiques de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e54f-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="5e54f-110">Les tests de contrainte déterminent si une application en contrainte peut récupérer après une défaillance et retourner normalement au comportement attendu.</span><span class="sxs-lookup"><span data-stu-id="5e54f-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="5e54f-111">En cas de stress, l’application n’est pas exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="5e54f-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="5e54f-112">Visual Studio 2019 est la dernière version de Visual Studio avec des fonctionnalités de test de charge.</span><span class="sxs-lookup"><span data-stu-id="5e54f-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="5e54f-113">Pour les clients nécessitant des outils de test de charge à l’avenir, nous vous recommandons d’autres outils, tels que Apache JMeter, Akamai CloudTest et BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="5e54f-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="5e54f-114">Pour plus d’informations, consultez les [notes de publication de Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="5e54f-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

<span data-ttu-id="5e54f-115">Le service de test de charge dans Azure DevOps se termine par 2020.</span><span class="sxs-lookup"><span data-stu-id="5e54f-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="5e54f-116">Pour plus d’informations, consultez [fin de vie du service de test de charge basé sur le Cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="5e54f-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="5e54f-117">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="5e54f-117">Visual Studio tools</span></span>

<span data-ttu-id="5e54f-118">Visual Studio permet aux utilisateurs de créer, développer et déboguer des tests de charge et de performances Web.</span><span class="sxs-lookup"><span data-stu-id="5e54f-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="5e54f-119">Une option est disponible pour créer des tests en enregistrant des actions dans un navigateur Web.</span><span class="sxs-lookup"><span data-stu-id="5e54f-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="5e54f-120">Pour plus d’informations sur la création, la configuration et l’exécution d’un projet de test de charge à [l’aide de Visual Studio 2017, consultez démarrage rapide: créer un projet de test de charge](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="5e54f-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="5e54f-121">Les tests de charge peuvent être configurés pour s’exécuter sur site ou dans le Cloud à l’aide d’Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="5e54f-121">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="5e54f-122">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="5e54f-122">Azure DevOps</span></span>

<span data-ttu-id="5e54f-123">Les séries de tests de charge peuvent être démarrées à l’aide du service [Azure DevOps test plans](/azure/devops/test/load-test/index?view=vsts) .</span><span class="sxs-lookup"><span data-stu-id="5e54f-123">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Page d’accueil du test de charge Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="5e54f-125">Le service prend en charge les formats de test suivants:</span><span class="sxs-lookup"><span data-stu-id="5e54f-125">The service supports the following test formats:</span></span>

* <span data-ttu-id="5e54f-126">Test Web &ndash; Visual Studio créé dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e54f-126">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="5e54f-127">Le trafic &ndash; http des Archives http capturées dans l’archive est relu pendant le test.</span><span class="sxs-lookup"><span data-stu-id="5e54f-127">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="5e54f-128">[Basé sur une URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Permet de spécifier des URL pour le test de charge, les types de demande, les en-têtes et les chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="5e54f-128">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="5e54f-129">Les paramètres d’exécution, tels que la durée, le modèle de charge et le nombre d’utilisateurs, peuvent être configurés.</span><span class="sxs-lookup"><span data-stu-id="5e54f-129">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="5e54f-130">[Apache jmeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="5e54f-130">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="5e54f-131">Portail Azure</span><span class="sxs-lookup"><span data-stu-id="5e54f-131">Azure portal</span></span>

<span data-ttu-id="5e54f-132">[Portail Azure permet de configurer et d’exécuter des tests de charge d’applications Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directement à partir de l’onglet **performances** de la app service dans portail Azure.</span><span class="sxs-lookup"><span data-stu-id="5e54f-132">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure App Service dans Portail Azure](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="5e54f-134">Le test peut être un test manuel avec une URL spécifiée ou un fichier de test Web Visual Studio, qui peut tester plusieurs URL.</span><span class="sxs-lookup"><span data-stu-id="5e54f-134">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Page nouveau test de performances sur Portail Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="5e54f-136">À la fin du test, les rapports générés affichent les caractéristiques de performances de l’application.</span><span class="sxs-lookup"><span data-stu-id="5e54f-136">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="5e54f-137">Voici quelques exemples de statistiques:</span><span class="sxs-lookup"><span data-stu-id="5e54f-137">Example statistics include:</span></span>

* <span data-ttu-id="5e54f-138">Temps de réponse moyen</span><span class="sxs-lookup"><span data-stu-id="5e54f-138">Average response time</span></span>
* <span data-ttu-id="5e54f-139">Débit maximal: demandes par seconde</span><span class="sxs-lookup"><span data-stu-id="5e54f-139">Max throughput: requests per second</span></span>
* <span data-ttu-id="5e54f-140">Pourcentage d’échec</span><span class="sxs-lookup"><span data-stu-id="5e54f-140">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="5e54f-141">Outils tiers</span><span class="sxs-lookup"><span data-stu-id="5e54f-141">Third-party tools</span></span>

<span data-ttu-id="5e54f-142">La liste suivante contient des outils de performances Web tiers avec différents ensembles de fonctionnalités:</span><span class="sxs-lookup"><span data-stu-id="5e54f-142">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="5e54f-143">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="5e54f-143">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="5e54f-144">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="5e54f-144">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="5e54f-145">Gatling</span><span class="sxs-lookup"><span data-stu-id="5e54f-145">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="5e54f-146">Caroubes</span><span class="sxs-lookup"><span data-stu-id="5e54f-146">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="5e54f-147">Surtension de l’ouest du vent</span><span class="sxs-lookup"><span data-stu-id="5e54f-147">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="5e54f-148">Netlingue</span><span class="sxs-lookup"><span data-stu-id="5e54f-148">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="5e54f-149">Vegeta</span><span class="sxs-lookup"><span data-stu-id="5e54f-149">Vegeta</span></span>](https://github.com/tsenart/vegeta)
