---
title: Le test de charge/contrainte ASP.NET Core
author: Jeremy-Meng
description: Décrit plusieurs outils et approches de test de charge et de stress d’applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 587df6e216943d3eeec779df4d0554dd0fc2fda0
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345426"
---
# <a name="load-and-stress-testing-aspnet-core"></a>Charger et de test ASP.NET Core de contrainte

Test de charge et tests de contrainte sont importantes pour garantir qu'une application web est performante et évolutive. Leurs objectifs diffèrent même ils partagent souvent des tests similaires.

**Tests de charge**: Teste si l’application peut gérer une charge spécifié d’utilisateurs pour un scénario certaines tout en répondant à l’objectif de réponse. L’application est exécutée dans des conditions normales.

**Tests de contrainte**: Tests application la stabilité lors de l’exécution sous les conditions d’utilisation extrêmes et souvent une longue période de temps :

* Charge utilisateur élevée – pics ou augmenter progressivement.
* Ressources informatiques limitées.  

En situation de stress, peut l’application récupérer et normalement revenir au comportement attendu ? En situation de stress, l’application est *pas* s’exécutent sous des conditions normales.

Visual Studio 2019 sera la dernière version de Visual Studio à proposer des fonctionnalités de test de charge. Pour les clients qui ont besoin d’outils de test de charge, nous leur recommandons d’utiliser d’autres outils de test de charge comme Apache JMeter, Akamai CloudTest ou Blazemeter. Pour plus d’informations, consultez le [Notes de publication Visual Studio 2019 Preview](/visualstudio/releases/2019/release-notes-preview#test-tools).

La tests de charge service dans Azure DevOps se termine dans 2020. Pour plus d’informations, consultez [nuage un test de charge principal de service de durée de vie](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio permet aux utilisateurs de créer, développer et déboguer les tests de charge et de performances web. Une option est disponible pour créer des tests à l’enregistrement des actions dans un navigateur web.

[Démarrage rapide : Créer un projet de test de charge](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) montre comment créer, configurer et exécuter un test de charge de projets à l’aide de Visual Studio 2017.

Pour plus d’informations, consultez [Ressources supplémentaires](#add).

Tests de charge peuvent être configurés pour exécuter en local ou exécuter dans le cloud à l’aide d’Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

Les séries de tests de charge peuvent être démarrées à l’aide de la [Azure DevOps Plans de Test](/azure/devops/test/load-test/index?view=vsts) service.

![](./load-tests/_static/azure-devops-load-test.png)

Le service prend en charge les types de format de test suivants :

- Visual Studio test – test web créé dans Visual Studio.
- Test basé sur une HTTP Archive – le trafic HTTP capturé à l’intérieur d’archive est relu pendant le test.
- [Test basé sur l’URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – permet de spécifier des URL pour charger le test, les types de demandes, les en-têtes et les chaînes de requête. Exécuter la définition des paramètres, tels que la durée, le modèle de charge, nombre d’utilisateurs, etc., peut être configuré.
- [Apache JMeter](https://jmeter.apache.org/) de test.

## <a name="azure-portal"></a>portail Azure

[Le portail Azure permet la configuration et l’exécution du test de charge des applications Web,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directement à partir de l’onglet performances du Service d’application dans le portail Azure.

![](./load-tests/_static/azure-appservice-perf-test.png)

Le test peut être un test manuel avec une URL spécifiée, ou un fichier de Test Web Visual Studio, qui peut tester plusieurs URL.

![](./load-tests/_static/azure-appservice-perf-test-config.png)

À la fin du test, les rapports sont générés pour afficher les caractéristiques de performances de l’application. Statistiques de l’exemple sont les suivantes :

- Temps de réponse moyen
- Débit max. : demandes par seconde
- Pourcentage d’échec

## <a name="third-party-tools"></a>Des outils tiers

La liste suivante contient des outils de performances web tierces avec différents ensembles de fonctionnalités :

- [Apache JMeter](https://jmeter.apache.org/) : Suite proposée complète d’outils de test de charge. Lié aux threads : besoin d’un thread par l’utilisateur.
- [AB - Apache HTTP server benchmark tool](https://httpd.apache.org/docs/2.4/programs/ab.html)
- [Gatling](https://gatling.io/) : Outil de bureau avec une interface graphique utilisateur et test enregistreurs. Plus performant que JMeter.
- [Locust.IO](https://locust.io/) : Pas délimités par des threads.

<a name="add"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Série de blogs de Test de charge](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) par Charles Sterling. Date d’effet, mais la plupart des rubriques sont toujours applicables.
