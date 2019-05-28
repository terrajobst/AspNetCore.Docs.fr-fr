---
title: Héberger et déployer Blazor
author: guardrex
description: Découvrez comment héberger et déployer des applications Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5def0356d13975211dd234f6a6a9f5a993d003b7
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223177"
---
# <a name="host-and-deploy-blazor"></a>Héberger et déployer Blazor

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publier l'application

Les applications sont publiées pour le déploiement dans la configuration Release.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Sélectionnez **Build** > **Publier {APPLICATION}** dans la barre de navigation.
1. Sélectionnez l’onglet *Cible de publication*. Pour publier localement, sélectionnez **Dossier**.
1. Acceptez l’emplacement par défaut dans le champ **Choisir un dossier** ou spécifiez un autre emplacement. Sélectionnez le bouton **Publier**.


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code/.NET Core CLI](#tab/visual-studio-code+netcore-cli)

Utilisez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) pour publier l’application avec une configuration Release :

```console
dotnet publish -c Release
```

---

La publication de l’application déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [crée](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement. Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application.

Une application Blazor côté client est publiée dans le dossier */bin/Release/{TARGET FRAMEWORK}/dist*. Une application Blazor côté serveur est publiée dans le dossier */bin/Release/{TARGET FRAMEWORK}/publish*.

Les ressources du dossier sont déployées sur le serveur web. Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.

## <a name="deployment"></a>Déploiement

Pour obtenir des conseils de déploiement, consultez les rubriques suivantes :

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Hébergement de Blazor serverless avec le stockage Azure

Les applications côté client Blazor peuvent être fournies par [Stockage Azure](https://azure.microsoft.com/services/storage/) en tant que contenu statique directement à partir d’un conteneur de stockage.

Pour plus d’informations, consultez [Héberger et déployer Blazor côté client (déploiement autonome) : Stockage Azure](xref:host-and-deploy/blazor/client-side#azure-storage).
