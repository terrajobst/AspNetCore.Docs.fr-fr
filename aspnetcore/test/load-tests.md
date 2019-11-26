---
title: ASP.NET Core le test de charge/stress
author: Jeremy-Meng
description: Découvrez plusieurs outils et approches notables pour le test de charge et les tests de stress ASP.NET Core les applications.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: edaa9e873e8e489f0c560c1736f81358ca1720d0
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289022"
---
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core le test de charge/stress

Les tests de charge et les tests de stress sont importants pour garantir qu’une application Web est performante et évolutive. Leurs objectifs sont différents même s’ils partagent souvent des tests similaires.

Les **tests de charge** &ndash; testent si l’application peut gérer une charge spécifiée d’utilisateurs pour un certain scénario tout en répondant à l’objectif de réponse. L’application est exécutée dans des conditions normales.

Les **tests de Stress** &ndash; la stabilité des applications de test lors de l’exécution dans des conditions extrêmes, souvent pendant une longue période de temps. Les tests mettent en place une charge utilisateur élevée, des pics ou une augmentation progressive de la charge, sur l’application, ou ils limitent les ressources informatiques de l’application.

Les tests de contrainte déterminent si une application en contrainte peut récupérer après une défaillance et retourner normalement au comportement attendu. En cas de stress, l’application n’est pas exécutée dans des conditions normales.

Visual Studio 2019 est la dernière version de Visual Studio avec des fonctionnalités de test de charge. Pour les clients nécessitant des outils de test de charge à l’avenir, nous vous recommandons d’autres outils, tels que Apache JMeter, Akamai CloudTest et BlazeMeter. Pour plus d’informations, consultez les [notes de publication de Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio permet aux utilisateurs de créer, développer et déboguer des tests de charge et de performances Web. Une option est disponible pour créer des tests en enregistrant des actions dans un navigateur Web.

Pour plus d’informations sur la création, la configuration et l’exécution de projets de test de charge à l’aide de Visual Studio 2017, consultez [démarrage rapide : créer un projet de test de charge](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).

Les tests de charge peuvent être configurés pour s’exécuter sur site ou dans le Cloud à l’aide d’Azure DevOps.

## <a name="third-party-tools"></a>Outils tiers

La liste suivante contient des outils de performances Web tiers avec différents ensembles de fonctionnalités :

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (AB)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Caroubes](https://locust.io/)
* [Surtension de l’ouest du vent](https://websurge.west-wind.com/)
* [Netlingue](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)
