---
title: Héberger et déployer Blazor
author: guardrex
description: Découvrez comment héberger et déployer des applications Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: c8a65b08582102af9129cf71ac4a108a905e49fc
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085531"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="6844c-103">Héberger et déployer Blazor</span><span class="sxs-lookup"><span data-stu-id="6844c-103">Host and deploy Blazor</span></span>

<span data-ttu-id="6844c-104">Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="6844c-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="6844c-105">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="6844c-105">Publish the app</span></span>

<span data-ttu-id="6844c-106">Les applications sont publiées pour le déploiement dans la configuration Release.</span><span class="sxs-lookup"><span data-stu-id="6844c-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6844c-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6844c-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6844c-108">Sélectionnez **Build** > **Publier {APPLICATION}** dans la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="6844c-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="6844c-109">Sélectionnez l’onglet *Cible de publication*.</span><span class="sxs-lookup"><span data-stu-id="6844c-109">Select the *publish target*.</span></span> <span data-ttu-id="6844c-110">Pour publier localement, sélectionnez **Dossier**.</span><span class="sxs-lookup"><span data-stu-id="6844c-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="6844c-111">Acceptez l’emplacement par défaut dans le champ **Choisir un dossier** ou spécifiez un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="6844c-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="6844c-112">Sélectionnez le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="6844c-112">Select the **Publish** button.</span></span>


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="6844c-113">Visual Studio Code/.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6844c-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="6844c-114">Utilisez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) pour publier l’application avec une configuration Release :</span><span class="sxs-lookup"><span data-stu-id="6844c-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="6844c-115">La publication de l’application déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [crée](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="6844c-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="6844c-116">Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.</span><span class="sxs-lookup"><span data-stu-id="6844c-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="6844c-117">Une application Blazor côté client est publiée dans le dossier */bin/Release/{TARGET FRAMEWORK}/dist*.</span><span class="sxs-lookup"><span data-stu-id="6844c-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/dist* folder.</span></span> <span data-ttu-id="6844c-118">Une application Blazor côté serveur est publiée dans le dossier */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="6844c-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="6844c-119">Les ressources du dossier sont déployées sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="6844c-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="6844c-120">Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.</span><span class="sxs-lookup"><span data-stu-id="6844c-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="6844c-121">Déploiement</span><span class="sxs-lookup"><span data-stu-id="6844c-121">Deployment</span></span>

<span data-ttu-id="6844c-122">Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="6844c-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
