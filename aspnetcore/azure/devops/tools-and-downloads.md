---
title: Outils et téléchargements - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Outils et téléchargements sont nécessaires pour les opérations de développement avec ASP.NET Core et Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 25ce311373b0aaddfa3bc2728c39e503acbca69d
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080637"
---
# <a name="tools-and-downloads"></a>Outils et téléchargements

Azure offre plusieurs interfaces pour l’approvisionnement et la gestion des ressources, telles que la [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)et Visual Studio. Ce guide adopte une approche minimaliste et utilise Azure Cloud Shell autant que possible afin de réduire les étapes requises. Toutefois, le portail Azure doit être utilisé pour certaines parties.

## <a name="prerequisites"></a>Prérequis

Les abonnements suivants sont requis :

* Azure &mdash; si vous n’avez pas un compte, [obtenir un essai gratuit](https://azure.microsoft.com/free/).
* Les Services Azure DevOps &mdash; votre abonnement d’Azure DevOps et votre organisation est créée dans le chapitre 4.
* GitHub &mdash; si vous n’avez pas un compte, [Inscrivez-vous gratuitement](https://github.com/join).

Les outils suivants sont requis :

* [GIT](https://git-scm.com/downloads) &mdash; une compréhension fondamentale de Git est recommandée pour ce guide. Examinez le [documentation Git](https://git-scm.com/doc), plus précisément [git distant](https://git-scm.com/docs/git-remote) et [git push](https://git-scm.com/docs/git-push).
* [SDK .NET core](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 ou version ultérieure est requis pour générer et exécuter l’exemple d’application. Si Visual Studio est installé avec le **.NET Core le développement multiplateforme** charge de travail, le SDK .NET Core est déjà installée.

    Vérifiez votre installation du SDK .NET Core. Ouvrez une invite de commandes et exécutez la commande suivante :

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Outils recommandés (Windows uniquement)

* [Visual Studio](https://visualstudio.microsoft.com)robustes outils Azure de fournir une interface graphique utilisateur pour la plupart des fonctionnalités décrites dans ce guide. N’importe quelle édition de Visual Studio fonctionnent, y compris l’édition Visual Studio Community gratuite. Les didacticiels sont écrits pour illustrer le développement, déploiement et DevOps avec et sans Visual Studio.

  Vérifiez que Visual Studio dispose les éléments suivants [charges de travail](/visualstudio/install/modify-visual-studio) installé :

  * Développement web et ASP.NET
  * Développement Azure
  * Développement multiplateforme .NET Core
