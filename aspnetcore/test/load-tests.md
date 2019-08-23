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
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core le test de charge/stress

Les tests de charge et les tests de stress sont importants pour garantir qu’une application Web est performante et évolutive. Leurs objectifs sont différents même s’ils partagent souvent des tests similaires.

**Tests de charge** &ndash; Teste si l’application peut gérer une charge d’utilisateurs spécifiée pour un certain scénario tout en répondant à l’objectif de réponse. L’application est exécutée dans des conditions normales.

**Tests de stress** &ndash; Testez la stabilité des applications en cas d’exécution dans des conditions extrêmes, souvent pendant une longue période de temps. Les tests mettent en place une charge utilisateur élevée, des pics ou une augmentation progressive de la charge, sur l’application, ou ils limitent les ressources informatiques de l’application.

Les tests de contrainte déterminent si une application en contrainte peut récupérer après une défaillance et retourner normalement au comportement attendu. En cas de stress, l’application n’est pas exécutée dans des conditions normales.

Visual Studio 2019 est la dernière version de Visual Studio avec des fonctionnalités de test de charge. Pour les clients nécessitant des outils de test de charge à l’avenir, nous vous recommandons d’autres outils, tels que Apache JMeter, Akamai CloudTest et BlazeMeter. Pour plus d’informations, consultez les [notes de publication de Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

Le service de test de charge dans Azure DevOps se termine par 2020. Pour plus d’informations, consultez [fin de vie du service de test de charge basé sur le Cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio permet aux utilisateurs de créer, développer et déboguer des tests de charge et de performances Web. Une option est disponible pour créer des tests en enregistrant des actions dans un navigateur Web.

Pour plus d’informations sur la création, la configuration et l’exécution d’un projet de test de charge à [l’aide de Visual Studio 2017, consultez démarrage rapide: créer un projet de test de charge](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).

Les tests de charge peuvent être configurés pour s’exécuter sur site ou dans le Cloud à l’aide d’Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

Les séries de tests de charge peuvent être démarrées à l’aide du service [Azure DevOps test plans](/azure/devops/test/load-test/index?view=vsts) .

![Page d’accueil du test de charge Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

Le service prend en charge les formats de test suivants:

* Test Web &ndash; Visual Studio créé dans Visual Studio.
* Le trafic &ndash; http des Archives http capturées dans l’archive est relu pendant le test.
* [Basé sur une URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Permet de spécifier des URL pour le test de charge, les types de demande, les en-têtes et les chaînes de requête. Les paramètres d’exécution, tels que la durée, le modèle de charge et le nombre d’utilisateurs, peuvent être configurés.
* [Apache jmeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Portail Azure

[Portail Azure permet de configurer et d’exécuter des tests de charge d’applications Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directement à partir de l’onglet **performances** de la app service dans portail Azure.

![Azure App Service dans Portail Azure](./load-tests/_static/azure-appservice-perf-test.png)

Le test peut être un test manuel avec une URL spécifiée ou un fichier de test Web Visual Studio, qui peut tester plusieurs URL.

![Page nouveau test de performances sur Portail Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

À la fin du test, les rapports générés affichent les caractéristiques de performances de l’application. Voici quelques exemples de statistiques:

* Temps de réponse moyen
* Débit maximal: demandes par seconde
* Pourcentage d’échec

## <a name="third-party-tools"></a>Outils tiers

La liste suivante contient des outils de performances Web tiers avec différents ensembles de fonctionnalités:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Caroubes](https://locust.io/)
* [Surtension de l’ouest du vent](https://websurge.west-wind.com/)
* [Netlingue](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)
