---
title: Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core
author: shirhatti
description: Découvrez la prise en charge du débogage des applications ASP.NET Core pendant l’exécution derrière IIS sur Windows Server.
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
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233077"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="30dc9-103">Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30dc9-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="30dc9-104">De [Sourabh Shirhatti](https://twitter.com/sshirhatti) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="30dc9-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="30dc9-105">Cet article décrit la prise en charge de [Visual Studio](https://www.visualstudio.com/vs/) pour le débogage des applications ASP.NET Core s’exécutant derrière IIS sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="30dc9-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="30dc9-106">Cette rubrique présente l’activation de cette fonctionnalité et la configuration d’un projet.</span><span class="sxs-lookup"><span data-stu-id="30dc9-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30dc9-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="30dc9-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="30dc9-108">Activer IIS</span><span class="sxs-lookup"><span data-stu-id="30dc9-108">Enable IIS</span></span>

1. <span data-ttu-id="30dc9-109">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="30dc9-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="30dc9-110">Cochez la case **Services IIS (Internet Information Services)**.</span><span class="sxs-lookup"><span data-stu-id="30dc9-110">Select the **Internet Information Services** check box.</span></span>

![Fonctionnalités Windows montrant la case à cocher Services IIS (Internet Information Services) avec un carré noir (pas une coche) indiquant que certaines des fonctionnalités IIS sont activées](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="30dc9-112">L’installation d’IIS peut nécessiter un redémarrage du système.</span><span class="sxs-lookup"><span data-stu-id="30dc9-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="30dc9-113">Configurer IIS</span><span class="sxs-lookup"><span data-stu-id="30dc9-113">Configure IIS</span></span>

<span data-ttu-id="30dc9-114">IIS doit avoir un site web configuré avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="30dc9-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="30dc9-115">Un nom d’hôte qui correspond au nom d’hôte de l’URL du profil de lancement de l’application</span><span class="sxs-lookup"><span data-stu-id="30dc9-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="30dc9-116">Une liaison pour le port 443 avec un certificat attribué</span><span class="sxs-lookup"><span data-stu-id="30dc9-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="30dc9-117">Par exemple, le **nom d’hôte** pour un site web ajouté est défini sur « localhost » (le profil de lancement utilise également « localhost » plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="30dc9-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="30dc9-118">Le port est défini sur « 443 » (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="30dc9-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="30dc9-119">Le **certificat de développement IIS Express** est attribué au site web, mais tout certificat valide convient :</span><span class="sxs-lookup"><span data-stu-id="30dc9-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Fenêtre Ajouter un site web dans IIS, montrant la liaison définie pour localhost sur le port 443 avec un certificat attribué.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="30dc9-121">Si l’installation d’IIS a déjà un **Site web par défaut** avec un nom d’hôte qui correspond au nom d’hôte de l’URL du profil de lancement de l’application :</span><span class="sxs-lookup"><span data-stu-id="30dc9-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="30dc9-122">Ajoutez une liaison de port pour le port 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="30dc9-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="30dc9-123">Attribuez un certificat valide au site web.</span><span class="sxs-lookup"><span data-stu-id="30dc9-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="30dc9-124">Activer la prise en charge d’IIS pendant le développement dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30dc9-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="30dc9-125">Lancez le programme d’installation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30dc9-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="30dc9-126">Sélectionnez le composant **Prise en charge d’IIS pendant le développement**.</span><span class="sxs-lookup"><span data-stu-id="30dc9-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="30dc9-127">Le composant apparaît comme facultatif dans le panneau **Résumé** pour la charge de travail **Développement web et ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="30dc9-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="30dc9-128">Le composant installe le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), qui est un module IIS natif nécessaire pour exécuter des applications ASP.NET Core derrière IIS dans une configuration de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="30dc9-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modification des fonctionnalités de Visual Studio : l’onglet Charges de travail est sélectionné.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="30dc9-132">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="30dc9-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="30dc9-133">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="30dc9-133">HTTPS redirection</span></span>

<span data-ttu-id="30dc9-134">Pour un nouveau projet, cochez la case **Configurer pour HTTPS** dans la fenêtre **Nouvelle application web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="30dc9-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Fenêtre Nouvelle application web ASP.NET Core avec la case Configurer pour HTTPS cochée.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="30dc9-136">Dans un projet existant, utilisez le middleware (intergiciel) de redirection HTTPS dans `Startup.Configure` en appelant la méthode d’extension [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) :</span><span class="sxs-lookup"><span data-stu-id="30dc9-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="30dc9-137">Profil de lancement IIS</span><span class="sxs-lookup"><span data-stu-id="30dc9-137">IIS launch profile</span></span>

<span data-ttu-id="30dc9-138">Créez un profil de lancement pour ajouter la prise en charge d’IIS pendant le développement :</span><span class="sxs-lookup"><span data-stu-id="30dc9-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="30dc9-139">Pour **Profil**, sélectionnez le bouton **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="30dc9-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="30dc9-140">Nommez le profil « IIS » dans la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="30dc9-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="30dc9-141">Sélectionnez **OK** pour créer le profil.</span><span class="sxs-lookup"><span data-stu-id="30dc9-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="30dc9-142">Pour le paramètre **Lancer**, sélectionnez **IIS** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="30dc9-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="30dc9-143">Cochez la case **Lancer le navigateur** et indiquez l’URL du point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="30dc9-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="30dc9-144">Utilisez le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="30dc9-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="30dc9-145">Cet exemple utilise `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="30dc9-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="30dc9-146">Dans la section **Variables d’environnement**, sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="30dc9-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="30dc9-147">Indiquez une variable d’environnement ayant pour clé `ASPNETCORE_ENVIRONMENT` et pour valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="30dc9-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="30dc9-148">Dans la zone **Paramètres du serveur web**, définissez **l’URL de l’application**.</span><span class="sxs-lookup"><span data-stu-id="30dc9-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="30dc9-149">Cet exemple utilise `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="30dc9-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="30dc9-150">Enregistrez le profil.</span><span class="sxs-lookup"><span data-stu-id="30dc9-150">Save the profile.</span></span>

![Fenêtre de propriétés du projet avec l’onglet Débogage sélectionné.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="30dc9-155">Vous pouvez aussi ajouter manuellement un profil de lancement au fichier [launchSettings.json](http://json.schemastore.org/launchsettings) dans l’application :</span><span class="sxs-lookup"><span data-stu-id="30dc9-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="30dc9-156">Exécuter le projet</span><span class="sxs-lookup"><span data-stu-id="30dc9-156">Run the project</span></span>

<span data-ttu-id="30dc9-157">Dans l’interface utilisateur de Visual Studio, définissez le bouton Exécuter sur le profil **IIS** et sélectionnez le bouton pour démarrer l’application :</span><span class="sxs-lookup"><span data-stu-id="30dc9-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Bouton Exécuter dans la barre d’outils de Visual Studio défini sur le profil «IIS »](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="30dc9-159">Visual Studio peut demander un redémarrage si vous ne l’exécutez pas en tant qu’administrateur.</span><span class="sxs-lookup"><span data-stu-id="30dc9-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="30dc9-160">Si vous y êtes invité, redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30dc9-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="30dc9-161">Si vous utilisez un certificat de développement non approuvé, le navigateur peut vous amener à créer une exception pour ce certificat.</span><span class="sxs-lookup"><span data-stu-id="30dc9-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30dc9-162">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="30dc9-162">Additional resources</span></span>

* [<span data-ttu-id="30dc9-163">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="30dc9-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="30dc9-164">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30dc9-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="30dc9-165">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30dc9-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="30dc9-166">Appliquer HTTPS</span><span class="sxs-lookup"><span data-stu-id="30dc9-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
