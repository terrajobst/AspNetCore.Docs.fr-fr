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
# <a name="host-and-deploy-razor-components-and-blazor"></a><span data-ttu-id="c5d6d-103">Héberger et déployer des composants Razor et Blazor</span><span class="sxs-lookup"><span data-stu-id="c5d6d-103">Host and deploy Razor Components and Blazor</span></span>

<span data-ttu-id="c5d6d-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c5d6d-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="c5d6d-105">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="c5d6d-105">Publish the app</span></span>

<span data-ttu-id="c5d6d-106">Les applications sont publiées pour le déploiement dans la configuration Release avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="c5d6d-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="c5d6d-107">Un environnement de développement intégré (IDE) peut gérer l’exécution de la commande `dotnet publish` automatiquement à l’aide de ses fonctionnalités de publication intégrées. Ainsi, en fonction des outils de développement utilisés, il ne sera peut-être pas nécessaire d’exécuter manuellement la commande à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="c5d6d-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

`dotnet publish` <span data-ttu-id="c5d6d-108">déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [génère](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="c5d6d-108">triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="c5d6d-109">Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.</span><span class="sxs-lookup"><span data-stu-id="c5d6d-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="c5d6d-110">Le déploiement est créé dans le dossier */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="c5d6d-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="c5d6d-111">Les ressources dans le dossier *publish* sont déployées sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="c5d6d-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="c5d6d-112">Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.</span><span class="sxs-lookup"><span data-stu-id="c5d6d-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="c5d6d-113">Déploiement</span><span class="sxs-lookup"><span data-stu-id="c5d6d-113">Deployment</span></span>

<span data-ttu-id="c5d6d-114">Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="c5d6d-114">For deployment guidance, see the following topics:</span></span>

* [<span data-ttu-id="c5d6d-115">Blazor côté client</span><span class="sxs-lookup"><span data-stu-id="c5d6d-115">Client-side Blazor</span></span>](xref:host-and-deploy/razor-components-blazor/blazor)
* [<span data-ttu-id="c5d6d-116">Composants Razor ASP.NET Core côté serveur</span><span class="sxs-lookup"><span data-stu-id="c5d6d-116">Server-side ASP.NET Core Razor Components</span></span>](xref:host-and-deploy/razor-components-blazor/razor-components)
