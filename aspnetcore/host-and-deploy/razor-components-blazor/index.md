---
title: Héberger et déployer des composants Razor et Blazor
author: guardrex
description: Découvrez comment héberger et déployer des composants Razor et des applications Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070306"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a>Héberger et déployer des composants Razor et Blazor

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

* [Blazor côté client](xref:host-and-deploy/razor-components-blazor/blazor)
* [Composants Razor ASP.NET Core côté serveur](xref:host-and-deploy/razor-components-blazor/razor-components)
