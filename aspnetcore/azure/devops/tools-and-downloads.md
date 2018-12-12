---
title: Outils et téléchargements - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Outils et téléchargements sont nécessaires pour les opérations de développement avec ASP.NET Core et Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 0c64e723f1b912323103f201a66c1edaeccdcc2d
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284344"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="4c9ad-103">Outils et téléchargements</span><span class="sxs-lookup"><span data-stu-id="4c9ad-103">Tools and downloads</span></span>

<span data-ttu-id="4c9ad-104">Azure offre plusieurs interfaces pour l’approvisionnement et la gestion des ressources, telles que la [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="4c9ad-105">Ce guide adopte une approche minimaliste et utilise Azure Cloud Shell autant que possible afin de réduire les étapes requises.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="4c9ad-106">Toutefois, le portail Azure doit être utilisé pour certaines parties.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c9ad-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="4c9ad-107">Prerequisites</span></span>

<span data-ttu-id="4c9ad-108">Les abonnements suivants sont requis :</span><span class="sxs-lookup"><span data-stu-id="4c9ad-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="4c9ad-109">Azure &mdash; si vous n’avez pas un compte, [obtenir un essai gratuit](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="4c9ad-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="4c9ad-110">Les Services Azure DevOps &mdash; votre abonnement d’Azure DevOps et votre organisation est créée dans le chapitre 4.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="4c9ad-111">GitHub &mdash; si vous n’avez pas un compte, [Inscrivez-vous gratuitement](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="4c9ad-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="4c9ad-112">Les outils suivants sont requis :</span><span class="sxs-lookup"><span data-stu-id="4c9ad-112">The following tools are required:</span></span>

* <span data-ttu-id="4c9ad-113">[GIT](https://git-scm.com/downloads) &mdash; une compréhension fondamentale de Git est recommandée pour ce guide.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="4c9ad-114">Examinez le [documentation Git](https://git-scm.com/doc), plus précisément [git distant](https://git-scm.com/docs/git-remote) et [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="4c9ad-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="4c9ad-115">[SDK .NET core](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 ou version ultérieure est requis pour générer et exécuter l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="4c9ad-116">Si Visual Studio est installé avec le **.NET Core le développement multiplateforme** charge de travail, le SDK .NET Core est déjà installée.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="4c9ad-117">Vérifiez votre installation du SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="4c9ad-118">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4c9ad-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="4c9ad-119">Outils recommandés (Windows uniquement)</span><span class="sxs-lookup"><span data-stu-id="4c9ad-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="4c9ad-120">[Visual Studio](https://www.visualstudio.com/)robustes outils Azure de fournir une interface graphique utilisateur pour la plupart des fonctionnalités décrites dans ce guide.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="4c9ad-121">N’importe quelle édition de Visual Studio fonctionnent, y compris l’édition Visual Studio Community gratuite.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="4c9ad-122">Les didacticiels sont écrits pour illustrer le développement, déploiement et DevOps avec et sans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c9ad-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="4c9ad-123">Vérifiez que Visual Studio dispose les éléments suivants [charges de travail](/visualstudio/install/modify-visual-studio) installé :</span><span class="sxs-lookup"><span data-stu-id="4c9ad-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="4c9ad-124">Développement web et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4c9ad-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="4c9ad-125">Développement Azure</span><span class="sxs-lookup"><span data-stu-id="4c9ad-125">Azure development</span></span>
  * <span data-ttu-id="4c9ad-126">Développement multiplateforme .NET Core</span><span class="sxs-lookup"><span data-stu-id="4c9ad-126">.NET Core cross-platform development</span></span>
