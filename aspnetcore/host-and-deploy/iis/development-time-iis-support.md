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
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="98493-103">Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98493-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="98493-104">De [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="98493-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="98493-105">Cet article décrit [Visual Studio](https://www.visualstudio.com/vs/) prend en charge pour le débogage des applications ASP.NET Core prend du retard IIS sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="98493-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="98493-106">Cette rubrique décrit l’activation de cette fonctionnalité et la configuration d’un projet.</span><span class="sxs-lookup"><span data-stu-id="98493-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98493-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="98493-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="98493-108">Activer IIS</span><span class="sxs-lookup"><span data-stu-id="98493-108">Enable IIS</span></span>

<span data-ttu-id="98493-109">Activez le service IIS.</span><span class="sxs-lookup"><span data-stu-id="98493-109">Enable IIS.</span></span> <span data-ttu-id="98493-110">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="98493-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="98493-111">Cochez la case **Services IIS (Internet Information Services)**.</span><span class="sxs-lookup"><span data-stu-id="98493-111">Select the **Internet Information Services** checkbox.</span></span>

![Fonctionnalités Windows montrant la case à cocher Services IIS (Internet Information Services) avec un carré noir (pas une coche) indiquant que certaines des fonctionnalités IIS sont activées](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="98493-113">Si l’installation d’IIS requiert un redémarrage, redémarrez le système.</span><span class="sxs-lookup"><span data-stu-id="98493-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="98493-114">Activer la prise en charge d’IIS lors du développement</span><span class="sxs-lookup"><span data-stu-id="98493-114">Enable development-time IIS support</span></span>

<span data-ttu-id="98493-115">Lancez le programme d’installation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="98493-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="98493-116">Sélectionnez le **IIS prend en charge des temps de développement** composant.</span><span class="sxs-lookup"><span data-stu-id="98493-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="98493-117">Le composant est répertorié comme facultatifs dans la **Résumé** panneau pour le **développement web ASP.NET et** la charge de travail.</span><span class="sxs-lookup"><span data-stu-id="98493-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="98493-118">Cette commande installe le [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), qui est un module IIS natif requis pour exécuter des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98493-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Modification des fonctionnalités de Visual Studio : l’onglet Charges de travail est sélectionné.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="98493-122">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="98493-122">Configure the project</span></span>

<span data-ttu-id="98493-123">Créez un nouveau profil de lancement pour ajouter la prise en charge d’IIS lors du développement.</span><span class="sxs-lookup"><span data-stu-id="98493-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="98493-124">Dans **l’Explorateur de solutions** de Visual Studio, cliquez avec le bouton droit sur le projet et sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="98493-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="98493-125">Sélectionnez l’onglet **Débogage**. Sélectionnez **IIS** dans la liste déroulante **Lancer**.</span><span class="sxs-lookup"><span data-stu-id="98493-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="98493-126">Vérifiez que la fonctionnalité **Lancer le navigateur** est activée avec l’URL correcte.</span><span class="sxs-lookup"><span data-stu-id="98493-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Fenêtre de propriétés du projet avec l’onglet Débogage sélectionné.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="98493-131">Vous pouvez également ajouter manuellement un profil de lancement pour les [launchSettings.json](http://json.schemastore.org/launchsettings) fichier dans l’application :</span><span class="sxs-lookup"><span data-stu-id="98493-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="98493-132">Visual Studio peut demander un redémarrage si ne pas en cours d’exécution en tant qu’administrateur.</span><span class="sxs-lookup"><span data-stu-id="98493-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="98493-133">Si vous y êtes invité, redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="98493-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="98493-134">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="98493-134">Additional resources</span></span>

* [<span data-ttu-id="98493-135">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="98493-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="98493-136">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98493-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="98493-137">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98493-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
