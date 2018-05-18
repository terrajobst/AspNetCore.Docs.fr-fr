---
title: Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core
author: shirhatti
description: Découvrez la prise en charge pour le débogage des applications ASP.NET Core lors de l’exécution derrière IIS sur Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core

Par [Sourabh Shirhatti](https://twitter.com/sshirhatti) et [Luke Latham](https://github.com/guardrex)

Cet article décrit [Visual Studio](https://www.visualstudio.com/vs/) prend en charge pour le débogage des applications ASP.NET Core prend du retard IIS sur Windows Server. Cette rubrique décrit l’activation de cette fonctionnalité et la configuration d’un projet.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Activer IIS

1. Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).
1. Sélectionnez le **Internet Information Services** case à cocher.

![Affichage des fonctionnalités de Windows Internet Information Services case à cocher vérifiée comme un carré noir (pas une coche) indiquant que certaines des fonctionnalités IIS sont activés](development-time-iis-support/_static/enable_iis.png)

L’installation d’IIS peut nécessiter un redémarrage du système.

## <a name="configure-iis"></a>Configurer IIS

IIS doit avoir un site Web configuré avec les éléments suivants :

* Nom un nom d’hôte qui correspond aux URL de profil de lancement de l’application d'hôte.
* Liaison pour le port 443 avec un certificat attribué.

Par exemple, le **nom d’hôte** pour un site Web ajouté est défini sur « localhost » (le profil de lancement utiliseront « localhost » plus loin dans cette rubrique). Le port est défini sur « 443 » (HTTPS). Le **IIS Express développement certificat** est attribué pour le site Web, mais n’importe quel certificat valide fonctionne :

![Ajouter la fenêtre du site Web dans IIS, affichage d’un ensemble de liaison pour localhost sur le port 443 avec un certificat attribué.](development-time-iis-support/_static/add-website-window.png)

Si l’installation d’IIS déjà a un **Site Web par défaut** avec un nom d’hôte qui correspond au nom d’hôte de l’application lancement profil URL :

* Ajoutez une liaison de port pour le port 443 (HTTPS).
* Attribuer un certificat valid pour le site Web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Activer la prise en charge IIS au moment du développement dans Visual Studio

1. Lancez le programme d’installation de Visual Studio.
1. Sélectionnez le **IIS prend en charge des temps de développement** composant. Le composant est répertorié comme facultatifs dans la **Résumé** panneau pour le **développement web ASP.NET et** la charge de travail. Le composant installe le [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), qui est un module IIS natif nécessaire pour exécuter des applications derrière IIS ASP.NET Core dans une configuration de proxy inverse.

![Modification des fonctionnalités de Visual Studio : l’onglet Charges de travail est sélectionné. Dans la section Web et cloud, le panneau Développement web et ASP.NET est sélectionné. Sur la droite dans la zone facultatif de l’écran récapitulatif, il est une case à cocher pour le moment du développement QU'IIS prend en charge.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurer le projet

### <a name="https-redirection"></a>Redirection HTTPS

Pour un nouveau projet, sélectionnez la case à cocher **configurer pour le protocole HTTPS** dans les **nouvelle Application ASP.NET Core Web** fenêtre :

![Nouvelle fenêtre d’Application ASP.NET Core Web avec la configuration de HTTPS case est cochée.](development-time-iis-support/_static/new-app.png)

Dans un projet existant, utilisez l’intergiciel (middleware) de HTTPS Redirection dans `Startup.Configure` en appelant le [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) méthode d’extension :

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Profil de lancement des services IIS

Créer un nouveau profil de lancement pour ajouter la prise en charge IIS au moment du développement :

1. Pour **profil**, sélectionnez le **nouveau** bouton. Nommez le profil « IIS » dans la fenêtre contextuelle. Sélectionnez **OK** pour créer le profil.
1. Pour le **lancer** paramètre, sélectionnez **IIS** dans la liste.
1. Sélectionnez la case à cocher **lancement navigateur** et indiquez l’URL de point de terminaison. Utiliser le protocole HTTPS. Cet exemple utilise `https://localhost/WebApplication1`.
1. Dans le **variables d’environnement** section, sélectionnez le **ajouter** bouton. Fournir une variable d’environnement avec une clé de `ASPNETCORE_ENVIRONMENT` et la valeur `Development`.
1. Dans le **paramètres du serveur Web** zone, définissez la **URL de l’application**. Cet exemple utilise `https://localhost/WebApplication1`.
1. Enregistrer le profil.

![Fenêtre de propriétés du projet avec l’onglet Débogage sélectionné. Les paramètres de profil et de lancement sont définis sur IIS. La fonctionnalité de navigateur de lancement est activée avec une adresse de https://localhost/WebApplication1. La même adresse est également fournie dans le champ URL de l’application de la zone de paramètres de serveur Web.](development-time-iis-support/_static/project_properties.png)

Vous pouvez également ajouter manuellement un profil de lancement pour les [launchSettings.json](http://json.schemastore.org/launchsettings) fichier dans l’application :

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Exécutez le projet

Dans l’interface utilisateur de Visual Studio, définissez le bouton exécuter sur le **IIS** de profil et sélectionnez le bouton pour démarrer l’application :

![Bouton exécuter dans la barre d’outils de Visual Studio défini pour le profil « IIS ».](development-time-iis-support/_static/toolbar.png)

Visual Studio peut demander un redémarrage si ne pas en cours d’exécution en tant qu’administrateur. Si vous y êtes invité, redémarrez Visual Studio.

Si un certificat non approuvé de développement est utilisé, le navigateur peut-être vous amener à créer une exception pour le certificat non approuvé.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Héberger ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index)
* [Introduction au Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Informations de référence sur la configuration du Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Appliquer HTTPS](xref:security/enforcing-ssl)
