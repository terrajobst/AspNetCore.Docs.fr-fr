---
title: Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core
author: shirhatti
description: Découvrez la prise en charge pour le débogage des applications ASP.NET Core lors de l’exécution derrière IIS sur Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core

De [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Cet article décrit [Visual Studio](https://www.visualstudio.com/vs/) prend en charge pour le débogage des applications ASP.NET Core prend du retard IIS sur Windows Server. Cette rubrique décrit l’activation de cette fonctionnalité et la configuration d’un projet.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Activer IIS

Activez le service IIS. Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran). Cochez la case **Services IIS (Internet Information Services)**.

![Fonctionnalités Windows montrant la case à cocher Services IIS (Internet Information Services) avec un carré noir (pas une coche) indiquant que certaines des fonctionnalités IIS sont activées](development-time-iis-support/_static/enable_iis.png)

Si l’installation d’IIS requiert un redémarrage, redémarrez le système.

## <a name="enable-development-time-iis-support"></a>Activer la prise en charge d’IIS lors du développement

Lancez le programme d’installation de Visual Studio. Sélectionnez le **IIS prend en charge des temps de développement** composant. Le composant est répertorié comme facultatifs dans la **Résumé** panneau pour le **développement web ASP.NET et** la charge de travail. Cette commande installe le [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), qui est un module IIS natif requis pour exécuter des applications ASP.NET Core.

![Modification des fonctionnalités de Visual Studio : l’onglet Charges de travail est sélectionné. Dans la section Web et cloud, le panneau Développement web et ASP.NET est sélectionné. Sur la droite dans la zone facultatif de l’écran récapitulatif, est une case à cocher pour le moment du développement QU'IIS prend en charge.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurer le projet

Créez un nouveau profil de lancement pour ajouter la prise en charge d’IIS lors du développement. Dans **l’Explorateur de solutions** de Visual Studio, cliquez avec le bouton droit sur le projet et sélectionnez **Propriétés**. Sélectionnez l’onglet **Débogage**. Sélectionnez **IIS** dans la liste déroulante **Lancer**. Vérifiez que la fonctionnalité **Lancer le navigateur** est activée avec l’URL correcte.

![Fenêtre de propriétés du projet avec l’onglet Débogage sélectionné. Les paramètres de profil et de lancement sont définis sur IIS. La fonctionnalité de navigateur de lancement est activée avec une adresse de http://localhost/WebApplication2. La même adresse est également fournie dans le champ URL de l’application de la zone Paramètres du serveur web avec Activer l’authentification anonyme activé.](development-time-iis-support/_static/project_properties.png)

Vous pouvez également ajouter manuellement un profil de lancement pour les [launchSettings.json](http://json.schemastore.org/launchsettings) fichier dans l’application :

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

Visual Studio peut demander un redémarrage si ne pas en cours d’exécution en tant qu’administrateur. Si vous y êtes invité, redémarrez Visual Studio.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index)
* [Introduction au Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Informations de référence sur la configuration du Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
