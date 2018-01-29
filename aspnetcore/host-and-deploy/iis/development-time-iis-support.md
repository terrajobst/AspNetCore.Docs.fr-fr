---
title: "Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core"
author: shirhatti
description: "Découvrez la prise en charge du débogage des applications ASP.NET Core lors de l’exécution derrière IIS sur Windows Server."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 264be49e8aff72f913c22508150e424541d49fa0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="e60a3-103">Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e60a3-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="e60a3-104">Par : [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="e60a3-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="e60a3-105">Cet article décrit la prise en charge de [Visual Studio](https://www.visualstudio.com/vs/) pour le débogage des applications ASP.NET Core s’exécutant derrière IIS sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="e60a3-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="e60a3-106">Cette rubrique décrit l’activation de cette fonctionnalité et la configuration d’un projet.</span><span class="sxs-lookup"><span data-stu-id="e60a3-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e60a3-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e60a3-107">Prerequisites</span></span>

* <span data-ttu-id="e60a3-108">Visual Studio (2017/version 15.3 ou ultérieure)</span><span class="sxs-lookup"><span data-stu-id="e60a3-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="e60a3-109">Charge de travail de développement ASP.NET et web *OU* la charge de travail de développement multiplateforme .NET Core</span><span class="sxs-lookup"><span data-stu-id="e60a3-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="e60a3-110">Activer IIS</span><span class="sxs-lookup"><span data-stu-id="e60a3-110">Enable IIS</span></span>

<span data-ttu-id="e60a3-111">Activez le service IIS.</span><span class="sxs-lookup"><span data-stu-id="e60a3-111">Enable IIS.</span></span> <span data-ttu-id="e60a3-112">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="e60a3-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="e60a3-113">Cochez la case **Services IIS (Internet Information Services)**.</span><span class="sxs-lookup"><span data-stu-id="e60a3-113">Select the **Internet Information Services** checkbox.</span></span>

![Fonctionnalités Windows montrant la case à cocher Services IIS (Internet Information Services) avec un carré noir (pas une coche) indiquant que certaines des fonctionnalités IIS sont activées](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="e60a3-115">Si l’installation d’IIS requiert un redémarrage, redémarrez le système.</span><span class="sxs-lookup"><span data-stu-id="e60a3-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="e60a3-116">Activer la prise en charge d’IIS lors du développement</span><span class="sxs-lookup"><span data-stu-id="e60a3-116">Enable development-time IIS support</span></span>

<span data-ttu-id="e60a3-117">Une fois IIS installé, lancez le programme d’installation de Visual Studio pour modifier l’installation existante de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e60a3-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="e60a3-118">Dans le programme d’installation, sélectionnez le composant **Prise en charge d’IIS pendant le développement**.</span><span class="sxs-lookup"><span data-stu-id="e60a3-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="e60a3-119">Le composant apparaît comme composant facultatif dans le panneau **Résumé** pour la charge de travail **Développement web et ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e60a3-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="e60a3-120">Ceci installe le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), qui est un module IIS natif nécessaire pour exécuter des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e60a3-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modification des fonctionnalités de Visual Studio : l’onglet Charges de travail est sélectionné.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="e60a3-124">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="e60a3-124">Configure the project</span></span>

<span data-ttu-id="e60a3-125">Créez un nouveau profil de lancement pour ajouter la prise en charge d’IIS lors du développement.</span><span class="sxs-lookup"><span data-stu-id="e60a3-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="e60a3-126">Dans **l’Explorateur de solutions** de Visual Studio, cliquez avec le bouton droit sur le projet et sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="e60a3-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="e60a3-127">Sélectionnez l’onglet **Débogage**. Sélectionnez **IIS** dans la liste déroulante **Lancer**.</span><span class="sxs-lookup"><span data-stu-id="e60a3-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="e60a3-128">Vérifiez que la fonctionnalité **Lancer le navigateur** est activée avec l’URL correcte.</span><span class="sxs-lookup"><span data-stu-id="e60a3-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Fenêtre de propriétés du projet avec l’onglet Débogage sélectionné.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="e60a3-133">Vous pouvez également ajouter manuellement un profil de lancement pour les [launchSettings.json](http://json.schemastore.org/launchsettings) fichier dans l’application :</span><span class="sxs-lookup"><span data-stu-id="e60a3-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="e60a3-134">Visual Studio peut demander un redémarrage si ne pas en cours d’exécution en tant qu’administrateur.</span><span class="sxs-lookup"><span data-stu-id="e60a3-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="e60a3-135">Si vous y êtes invité, redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e60a3-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="e60a3-136">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="e60a3-136">Congratulations!</span></span> <span data-ttu-id="e60a3-137">À ce stade, le projet est configuré pour la prise en charge au moment du développement IIS.</span><span class="sxs-lookup"><span data-stu-id="e60a3-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e60a3-138">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e60a3-138">Additional resources</span></span>

* [<span data-ttu-id="e60a3-139">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="e60a3-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e60a3-140">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e60a3-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="e60a3-141">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e60a3-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
