---
title: DevOps avec ASP.NET Core et Azure | Outils et téléchargements
author: CamSoper
description: Un guide qui fournit des conseils de bout en bout sur la création d’un pipeline DevOps pour une application ASP.NET Core hébergée dans Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a63e97d9ab9eb0ed2fbd30e8c2e033f0c048d33e
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312299"
---
# <a name="tools-and-downloads"></a>Outils et téléchargements

Azure offre plusieurs interfaces pour l’approvisionnement et la gestion des ressources, telles que la [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)et Visual Studio. Ce guide adopte une approche minimaliste et utilise Azure Cloud Shell autant que possible afin de réduire les étapes requises. Toutefois, le portail Azure doit être utilisé pour certaines parties.

## <a name="prerequisites"></a>Prérequis

Les abonnements suivants sont requis :

* Azure &mdash; si vous n’avez pas un compte, [obtenir un essai gratuit](https://azure.microsoft.com/free/).
* Visual Studio Team Services (VSTS) &mdash; ce compte est créé dans le chapitre 4.
* GitHub &mdash; si vous n’avez pas un compte, [Inscrivez-vous gratuitement](https://github.com/join).

Les outils suivants sont requis :

* [GIT](https://git-scm.com/downloads) &mdash; une compréhension fondamentale de Git est recommandée pour ce guide. Examinez le [documentation Git](https://git-scm.com/doc), plus précisément [git distant](https://git-scm.com/docs/git-remote) et [git push](https://git-scm.com/docs/git-push).
* [SDK .NET core](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 ou version ultérieure est requis pour générer et exécuter l’exemple d’application. Si Visual Studio est installé avec le **.NET Core le développement multiplateforme** charge de travail, le SDK .NET Core est déjà installée.

    Vérifiez votre installation du SDK .NET Core. Ouvrez une invite de commandes et exécutez la commande suivante :

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Outils recommandés (Windows uniquement)

* [Visual Studio](https://www.visualstudio.com/)robustes outils Azure de fournir une interface graphique utilisateur pour la plupart des fonctionnalités décrites dans ce guide. N’importe quelle édition de Visual Studio fonctionnent, y compris l’édition Visual Studio Community gratuite. Les didacticiels sont écrits pour illustrer le développement, déploiement et DevOps avec et sans Visual Studio.

  Vérifiez que Visual Studio dispose les éléments suivants [charges de travail](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installé :

  * Développement web et ASP.NET
  * Développement Azure
  * Développement multiplateforme .NET Core
