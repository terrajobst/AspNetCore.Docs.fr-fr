---
title: Le test de charge/contrainte ASP.NET Core
author: Jeremy-Meng
description: Décrit plusieurs outils et approches de test de charge et de stress d’applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 0a53405cba19435a74b398ba42a05456c50bdc72
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099479"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="a3cb1-103">Charger et de test ASP.NET Core de contrainte</span><span class="sxs-lookup"><span data-stu-id="a3cb1-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="a3cb1-104">Test de charge et tests de contrainte sont importantes pour garantir qu'une application web est performante et évolutive.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="a3cb1-105">Leurs objectifs diffèrent même ils partagent souvent des tests similaires.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="a3cb1-106">**Tests de charge**: Teste si l’application peut gérer une charge spécifié d’utilisateurs pour un scénario certaines tout en répondant à l’objectif de réponse.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="a3cb1-107">L’application est exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="a3cb1-108">**Tests de contrainte**: Tests application la stabilité lors de l’exécution sous les conditions d’utilisation extrêmes et souvent une longue période de temps :</span><span class="sxs-lookup"><span data-stu-id="a3cb1-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="a3cb1-109">Charge utilisateur élevée – pics ou augmenter progressivement.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="a3cb1-110">Ressources informatiques limitées.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-110">Limited computing resources.</span></span>  

<span data-ttu-id="a3cb1-111">En situation de stress, peut l’application récupérer et normalement revenir au comportement attendu ?</span><span class="sxs-lookup"><span data-stu-id="a3cb1-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="a3cb1-112">L’application est exécutée dans des conditions normales.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-112">The app is run under normal conditions.</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="a3cb1-113">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="a3cb1-113">Visual Studio Tools</span></span>

<span data-ttu-id="a3cb1-114">Visual Studio permet aux utilisateurs de créer, développer et déboguer les tests de charge et de performances web.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-114">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="a3cb1-115">Une option est disponible pour créer des tests à l’enregistrement des actions dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-115">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="a3cb1-116">[Démarrage rapide : Créer un projet de test de charge](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) montre comment créer, configurer et exécuter un test de charge de projets à l’aide de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-116">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="a3cb1-117">Pour plus d’informations, consultez [Ressources supplémentaires](#add).</span><span class="sxs-lookup"><span data-stu-id="a3cb1-117">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="a3cb1-118">Tests de charge peuvent être configurés pour exécuter en local ou exécuter dans le cloud à l’aide d’Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-118">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="a3cb1-119">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="a3cb1-119">Azure DevOps</span></span>

<span data-ttu-id="a3cb1-120">Les séries de tests de charge peuvent être démarrées à l’aide de la [Azure DevOps Plans de Test](/azure/devops/test/load-test/index?view=vsts) service.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-120">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="a3cb1-121">Le service prend en charge les types de format de test suivants :</span><span class="sxs-lookup"><span data-stu-id="a3cb1-121">The service supports the following types of test format:</span></span>

- <span data-ttu-id="a3cb1-122">Visual Studio test – test web créé dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-122">Visual Studio test – web test created in Visual Studio.</span></span>
- <span data-ttu-id="a3cb1-123">Test basé sur une HTTP Archive – le trafic HTTP capturé à l’intérieur d’archive est relu pendant le test.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-123">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
- <span data-ttu-id="a3cb1-124">[Test basé sur l’URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – permet de spécifier des URL pour charger le test, les types de demandes, les en-têtes et les chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-124">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="a3cb1-125">Exécuter la définition des paramètres, tels que la durée, le modèle de charge, nombre d’utilisateurs, etc., peut être configuré.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-125">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
- <span data-ttu-id="a3cb1-126">[Apache JMeter](https://jmeter.apache.org/) de test.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-126">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="a3cb1-127">portail Azure</span><span class="sxs-lookup"><span data-stu-id="a3cb1-127">Azure portal</span></span>

<span data-ttu-id="a3cb1-128">[Le portail Azure permet la configuration et l’exécution du test de charge des applications Web,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directement à partir de l’onglet performances du Service d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-128">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="a3cb1-129">Le test peut être un test manuel avec une URL spécifiée, ou un fichier de Test Web Visual Studio, qui peut tester plusieurs URL.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-129">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="a3cb1-130">À la fin du test, les rapports sont générés pour afficher les caractéristiques de performances de l’application.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-130">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="a3cb1-131">Statistiques de l’exemple sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3cb1-131">Example statistics include:</span></span>

- <span data-ttu-id="a3cb1-132">Temps de réponse moyen</span><span class="sxs-lookup"><span data-stu-id="a3cb1-132">Average response time</span></span>
- <span data-ttu-id="a3cb1-133">Débit max. : demandes par seconde</span><span class="sxs-lookup"><span data-stu-id="a3cb1-133">Max throughput: requests per second</span></span>
- <span data-ttu-id="a3cb1-134">Pourcentage d’échec</span><span class="sxs-lookup"><span data-stu-id="a3cb1-134">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="a3cb1-135">Des outils tiers</span><span class="sxs-lookup"><span data-stu-id="a3cb1-135">Third-party Tools</span></span>

<span data-ttu-id="a3cb1-136">La liste suivante contient des outils de performances web tierces avec différents ensembles de fonctionnalités :</span><span class="sxs-lookup"><span data-stu-id="a3cb1-136">The following list contains third-party web performance tools with various feature sets:</span></span>

- <span data-ttu-id="a3cb1-137">[Apache JMeter](https://jmeter.apache.org/) : Suite proposée complète d’outils de test de charge.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-137">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="a3cb1-138">Lié aux threads : besoin d’un thread par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-138">Thread-bound: need one thread per user.</span></span>
- [<span data-ttu-id="a3cb1-139">AB - Apache HTTP server benchmark tool</span><span class="sxs-lookup"><span data-stu-id="a3cb1-139">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
- <span data-ttu-id="a3cb1-140">[Gatling](https://gatling.io/) : Outil de bureau avec une interface graphique utilisateur et test enregistreurs.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-140">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="a3cb1-141">Plus performant que JMeter.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-141">More performant than JMeter.</span></span>
- <span data-ttu-id="a3cb1-142">[Locust.IO](https://locust.io/) : Pas délimités par des threads.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-142">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>
## <a name="additional-resources"></a><span data-ttu-id="a3cb1-143">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a3cb1-143">Additional Resources</span></span>

<span data-ttu-id="a3cb1-144">[Série de blogs de Test de charge](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) par Charles Sterling.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-144">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="a3cb1-145">Date d’effet, mais la plupart des rubriques sont toujours applicables.</span><span class="sxs-lookup"><span data-stu-id="a3cb1-145">Dated but most of the topics are still relevant.</span></span>