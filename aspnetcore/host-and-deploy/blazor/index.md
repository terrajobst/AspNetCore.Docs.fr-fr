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
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="ea1f9-103">Héberger et déployer Blazor</span><span class="sxs-lookup"><span data-stu-id="ea1f9-103">Host and deploy Blazor</span></span>

<span data-ttu-id="ea1f9-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ea1f9-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="ea1f9-105">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="ea1f9-105">Publish the app</span></span>

<span data-ttu-id="ea1f9-106">Les applications sont publiées pour le déploiement dans la configuration Release avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="ea1f9-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="ea1f9-107">Un environnement de développement intégré (IDE) peut gérer l’exécution de la commande `dotnet publish` automatiquement à l’aide de ses fonctionnalités de publication intégrées. Ainsi, en fonction des outils de développement utilisés, il ne sera peut-être pas nécessaire d’exécuter manuellement la commande à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="ea1f9-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="ea1f9-108">`dotnet publish` déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [génère](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="ea1f9-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="ea1f9-109">Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.</span><span class="sxs-lookup"><span data-stu-id="ea1f9-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="ea1f9-110">Le déploiement est créé dans le dossier */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="ea1f9-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="ea1f9-111">Les ressources dans le dossier *publish* sont déployées sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="ea1f9-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="ea1f9-112">Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.</span><span class="sxs-lookup"><span data-stu-id="ea1f9-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="ea1f9-113">Déploiement</span><span class="sxs-lookup"><span data-stu-id="ea1f9-113">Deployment</span></span>

<span data-ttu-id="ea1f9-114">Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="ea1f9-114">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
