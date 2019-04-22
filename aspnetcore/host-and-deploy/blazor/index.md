---
title: Héberger et déployer Blazor
author: guardrex
description: Découvrez comment héberger et déployer des applications Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: a7739e2b240d7fd6c85e68105892c802228ebeb5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614565"
---
# <a name="host-and-deploy-blazor"></a>Héberger et déployer Blazor

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publier l'application

Les applications sont publiées pour le déploiement dans la configuration Release avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish). Un environnement de développement intégré (IDE) peut gérer l’exécution de la commande `dotnet publish` automatiquement à l’aide de ses fonctionnalités de publication intégrées. Ainsi, en fonction des outils de développement utilisés, il ne sera peut-être pas nécessaire d’exécuter manuellement la commande à partir d’une invite de commandes.

```console
dotnet publish -c Release
```

`dotnet publish` déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [génère](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement. Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application. Le déploiement est créé dans le dossier */bin/Release/{TARGET FRAMEWORK}/publish*.

Les ressources dans le dossier *publish* sont déployées sur le serveur web. Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.

## <a name="deployment"></a>Déploiement

Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
