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
# <a name="devops-with-aspnet-core-and-azure"></a>DevOps avec ASP.NET Core et Azure

Bienvenue dans le guide du cycle de vie du développement Azure pour .NET ! Ce guide présente les concepts de base de la création d’un cycle de vie du développement autour d’Azure avec les outils et les processus .NET. Après avoir terminé ce guide, vous pourrez tirer parti d’une chaîne d’outils DevOps arrivés à maturation.

## <a name="who-this-guide-is-for"></a>Audience à laquelle ce guide est destiné

Vous devez être un développeur ASP.NET expérimenté (niveau 200-300). Vous n’avez pas besoin de connaissances sur Azure, car nous couvrons cet aspect dans cette introduction. Ce guide peut également être utile aux ingénieurs DevOps qui se consacrent davantage aux opérations qu’au développement.

Ce guide cible les développeurs Windows. Cependant, Linux et macOS sont entièrement pris en charge par .NET Core. Pour adapter ce guide à Linux/macOS, consultez les indications faisant état des différences pour Linux/macOS.

## <a name="what-this-guide-doesnt-cover"></a>Ce que ce guide ne couvre pas

Ce guide est centré sur une expérience de déploiement continu de bout en bout pour les développeurs .NET. Il ne s’agit pas d’un guide exhaustif sur Azure, et il n’aborde pas de façon détaillée les API .NET pour les services Azure. L’accent est mis sur tout ce qui a trait à l’intégration continue, au déploiement, à la surveillance et au débogage. Vous pouvez trouver des recommandations pour les étapes suivantes vers la fin du guide. Les services de plateforme Azure qui sont utiles aux développeurs ASP.NET figurent dans les suggestions.

## <a name="whats-in-this-guide"></a>Contenu de ce guide

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Outils et téléchargements](xref:azure/devops/tools-and-downloads)

Découvrez où vous procurer les outils utilisés dans ce guide.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[Déployer sur App Service](xref:azure/devops/deploy-to-app-service)

Découvrez les différentes méthodes de déploiement d’une application ASP.NET Core sur Azure App Service.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Intégration et déploiement continus](xref:azure/devops/cicd)

Créez une solution d’intégration et de déploiement continus de bout en bout pour votre application ASP.NET Core avec GitHub, VSTS et Azure.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[Surveiller et déboguer](xref:azure/devops/monitor)

Utilisez les outils d’Azure pour surveiller, dépanner et optimiser votre application.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Étapes suivantes](xref:azure/devops/next-steps)

Autres parcours d’apprentissage pour les développeurs ASP.NET Core découvrant Azure.

## <a name="acknowledgments"></a>Remerciements

Nous remercions tous les membres de la communauté .NET qui ont contribué à ce guide pour leurs suggestions pertinentes ! Nous aimerions en particulier remercier les membres suivants de la communauté, qui ont contribué à la révision finale de ce document :

* [Sam Wronski](https://www.youtube.com/c/worldofzerodevelopment)
* [Jeffrey Palermo](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a>Conclusion

Ce guide vous prépare à créer un cycle de vie de développement d’intégration continue construit autour d’ASP.NET Core et d’Azure App Service.

## <a name="additional-reading"></a>Lecture supplémentaire

* [Qu’est-ce que le cloud computing ?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Exemples de cloud computing](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [Qu’est-ce que IaaS ?](https://azure.microsoft.com/overview/what-is-iaas/)
* [Qu’est-ce que PaaS ?](https://azure.microsoft.com/overview/what-is-paas/)
