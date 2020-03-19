---
title: Outils et téléchargements - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Outils et téléchargements sont nécessaires pour les opérations de développement avec ASP.NET Core et Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 9c1042dd48b9167209b46e97a09e011b80e2609c
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511143"
---
# <a name="tools-and-downloads"></a>Outils et téléchargements

Azure dispose de plusieurs interfaces pour la configuration et la gestion des ressources, telles que les [portail Azure](https://portal.azure.com), les [Azure CLI](/cli/azure/), les [Azure PowerShell](/powershell/azure/overview), les [Azure Cloud Shell](https://shell.azure.com/bash)et Visual Studio. Ce guide adopte une approche minimaliste et utilise Azure Cloud Shell autant que possible afin de réduire les étapes requises. Toutefois, le portail Azure doit être utilisé pour certaines parties.

## <a name="prerequisites"></a>Conditions préalables requises

Les abonnements suivants sont requis :

* &mdash; Azure si vous n’avez pas de compte, [procurez-vous une version d’évaluation gratuite](https://azure.microsoft.com/free/).
* Azure DevOps Services &mdash; votre abonnement et votre organisation Azure DevOps sont créés dans le chapitre 4.
* GitHub &mdash; si vous n’avez pas de compte, inscrivez-vous [gratuitement](https://github.com/join).

Les outils suivants sont requis :

* [Git](https://git-scm.com/downloads) &mdash; une compréhension fondamentale de git est recommandée pour ce guide. Consultez la [documentation git](https://git-scm.com/doc), en particulier [git remote](https://git-scm.com/docs/git-remote) et [git push](https://git-scm.com/docs/git-push).
* [Kit SDK .NET Core](https://dotnet.microsoft.com/download/) &mdash; version 2.1.300 ou ultérieure est nécessaire pour générer et exécuter l’exemple d’application. Si Visual Studio est installé avec la charge de travail de **développement multiplateforme .net Core** , le kit SDK .net Core est déjà installé.

    Vérifiez votre installation du SDK .NET Core. Ouvrez une invite de commandes et exécutez la commande suivante :

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Outils recommandés (Windows uniquement)

* Les outils Azure robustes de [Visual Studio](https://visualstudio.microsoft.com)fournissent une interface utilisateur graphique pour la plupart des fonctionnalités décrites dans ce guide. N’importe quelle édition de Visual Studio fonctionnent, y compris l’édition Visual Studio Community gratuite. Les didacticiels sont écrits pour illustrer le développement, déploiement et DevOps avec et sans Visual Studio.

  Vérifiez que les [charges de travail](/visualstudio/install/modify-visual-studio) suivantes sont installées sur Visual Studio :

  * Développement web et ASP.NET
  * Développement Azure
  * Développement multiplateforme .NET Core
