---
title: DevOps avec ASP.NET Core et Azure | Outils et téléchargements
author: CamSoper
description: Un guide qui fournit des conseils de bout en bout sur la création d’un pipeline DevOps pour une application ASP.NET Core hébergée dans Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 3cf99f4d497bf2edd8759ab9afdee66ad49fac3d
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42909968"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="c80a6-103">Outils et téléchargements</span><span class="sxs-lookup"><span data-stu-id="c80a6-103">Tools and downloads</span></span>

<span data-ttu-id="c80a6-104">Azure offre plusieurs interfaces pour l’approvisionnement et la gestion des ressources, telles que la [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c80a6-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="c80a6-105">Ce guide adopte une approche minimaliste et utilise Azure Cloud Shell autant que possible afin de réduire les étapes requises.</span><span class="sxs-lookup"><span data-stu-id="c80a6-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="c80a6-106">Toutefois, le portail Azure doit être utilisé pour certaines parties.</span><span class="sxs-lookup"><span data-stu-id="c80a6-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c80a6-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="c80a6-107">Prerequisites</span></span>

<span data-ttu-id="c80a6-108">Les abonnements suivants sont requis :</span><span class="sxs-lookup"><span data-stu-id="c80a6-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="c80a6-109">Azure &mdash; si vous n’avez pas un compte, [obtenir un essai gratuit](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c80a6-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="c80a6-110">Visual Studio Team Services (VSTS) &mdash; ce compte est créé dans le chapitre 4.</span><span class="sxs-lookup"><span data-stu-id="c80a6-110">Visual Studio Team Services (VSTS) &mdash; This account is created in Chapter 4.</span></span>
* <span data-ttu-id="c80a6-111">GitHub &mdash; si vous n’avez pas un compte, [Inscrivez-vous gratuitement](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="c80a6-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="c80a6-112">Les outils suivants sont requis :</span><span class="sxs-lookup"><span data-stu-id="c80a6-112">The following tools are required:</span></span>

* <span data-ttu-id="c80a6-113">[GIT](https://git-scm.com/downloads) &mdash; une compréhension fondamentale de Git est recommandée pour ce guide.</span><span class="sxs-lookup"><span data-stu-id="c80a6-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="c80a6-114">Examinez le [documentation Git](https://git-scm.com/doc), plus précisément [git distant](https://git-scm.com/docs/git-remote) et [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="c80a6-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="c80a6-115">[SDK .NET core](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 ou version ultérieure est requis pour générer et exécuter l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="c80a6-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="c80a6-116">Si Visual Studio est installé avec le **.NET Core le développement multiplateforme** charge de travail, le SDK .NET Core est déjà installée.</span><span class="sxs-lookup"><span data-stu-id="c80a6-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="c80a6-117">Vérifiez votre installation du SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c80a6-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="c80a6-118">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c80a6-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="c80a6-119">Outils recommandés (Windows uniquement)</span><span class="sxs-lookup"><span data-stu-id="c80a6-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="c80a6-120">[Visual Studio](https://www.visualstudio.com/)robustes outils Azure de fournir une interface graphique utilisateur pour la plupart des fonctionnalités décrites dans ce guide.</span><span class="sxs-lookup"><span data-stu-id="c80a6-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="c80a6-121">N’importe quelle édition de Visual Studio fonctionnent, y compris l’édition Visual Studio Community gratuite.</span><span class="sxs-lookup"><span data-stu-id="c80a6-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="c80a6-122">Les didacticiels sont écrits pour illustrer le développement, déploiement et DevOps avec et sans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c80a6-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="c80a6-123">Vérifiez que Visual Studio dispose les éléments suivants [charges de travail](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installé :</span><span class="sxs-lookup"><span data-stu-id="c80a6-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="c80a6-124">Développement web et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c80a6-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="c80a6-125">Développement Azure</span><span class="sxs-lookup"><span data-stu-id="c80a6-125">Azure development</span></span>
  * <span data-ttu-id="c80a6-126">Développement multiplateforme .NET Core</span><span class="sxs-lookup"><span data-stu-id="c80a6-126">.NET Core cross-platform development</span></span>
