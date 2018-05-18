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
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="cefc4-103">Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cefc4-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="cefc4-104">Par [Sourabh Shirhatti](https://twitter.com/sshirhatti) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cefc4-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cefc4-105">Cet article décrit [Visual Studio](https://www.visualstudio.com/vs/) prend en charge pour le débogage des applications ASP.NET Core prend du retard IIS sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="cefc4-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="cefc4-106">Cette rubrique décrit l’activation de cette fonctionnalité et la configuration d’un projet.</span><span class="sxs-lookup"><span data-stu-id="cefc4-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cefc4-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="cefc4-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="cefc4-108">Activer IIS</span><span class="sxs-lookup"><span data-stu-id="cefc4-108">Enable IIS</span></span>

1. <span data-ttu-id="cefc4-109">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="cefc4-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="cefc4-110">Sélectionnez le **Internet Information Services** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="cefc4-110">Select the **Internet Information Services** check box.</span></span>

![Affichage des fonctionnalités de Windows Internet Information Services case à cocher vérifiée comme un carré noir (pas une coche) indiquant que certaines des fonctionnalités IIS sont activés](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="cefc4-112">L’installation d’IIS peut nécessiter un redémarrage du système.</span><span class="sxs-lookup"><span data-stu-id="cefc4-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="cefc4-113">Configurer IIS</span><span class="sxs-lookup"><span data-stu-id="cefc4-113">Configure IIS</span></span>

<span data-ttu-id="cefc4-114">IIS doit avoir un site Web configuré avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="cefc4-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="cefc4-115">Nom un nom d’hôte qui correspond aux URL de profil de lancement de l’application d'hôte.</span><span class="sxs-lookup"><span data-stu-id="cefc4-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="cefc4-116">Liaison pour le port 443 avec un certificat attribué.</span><span class="sxs-lookup"><span data-stu-id="cefc4-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="cefc4-117">Par exemple, le **nom d’hôte** pour un site Web ajouté est défini sur « localhost » (le profil de lancement utiliseront « localhost » plus loin dans cette rubrique).</span><span class="sxs-lookup"><span data-stu-id="cefc4-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="cefc4-118">Le port est défini sur « 443 » (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cefc4-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="cefc4-119">Le **IIS Express développement certificat** est attribué pour le site Web, mais n’importe quel certificat valide fonctionne :</span><span class="sxs-lookup"><span data-stu-id="cefc4-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Ajouter la fenêtre du site Web dans IIS, affichage d’un ensemble de liaison pour localhost sur le port 443 avec un certificat attribué.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="cefc4-121">Si l’installation d’IIS déjà a un **Site Web par défaut** avec un nom d’hôte qui correspond au nom d’hôte de l’application lancement profil URL :</span><span class="sxs-lookup"><span data-stu-id="cefc4-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="cefc4-122">Ajoutez une liaison de port pour le port 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="cefc4-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="cefc4-123">Attribuer un certificat valid pour le site Web.</span><span class="sxs-lookup"><span data-stu-id="cefc4-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="cefc4-124">Activer la prise en charge IIS au moment du développement dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cefc4-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="cefc4-125">Lancez le programme d’installation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cefc4-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="cefc4-126">Sélectionnez le **IIS prend en charge des temps de développement** composant.</span><span class="sxs-lookup"><span data-stu-id="cefc4-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="cefc4-127">Le composant est répertorié comme facultatifs dans la **Résumé** panneau pour le **développement web ASP.NET et** la charge de travail.</span><span class="sxs-lookup"><span data-stu-id="cefc4-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="cefc4-128">Le composant installe le [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), qui est un module IIS natif nécessaire pour exécuter des applications derrière IIS ASP.NET Core dans une configuration de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="cefc4-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modification des fonctionnalités de Visual Studio : l’onglet Charges de travail est sélectionné.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="cefc4-132">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="cefc4-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="cefc4-133">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="cefc4-133">HTTPS redirection</span></span>

<span data-ttu-id="cefc4-134">Pour un nouveau projet, sélectionnez la case à cocher **configurer pour le protocole HTTPS** dans les **nouvelle Application ASP.NET Core Web** fenêtre :</span><span class="sxs-lookup"><span data-stu-id="cefc4-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nouvelle fenêtre d’Application ASP.NET Core Web avec la configuration de HTTPS case est cochée.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="cefc4-136">Dans un projet existant, utilisez l’intergiciel (middleware) de HTTPS Redirection dans `Startup.Configure` en appelant le [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) méthode d’extension :</span><span class="sxs-lookup"><span data-stu-id="cefc4-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="cefc4-137">Profil de lancement des services IIS</span><span class="sxs-lookup"><span data-stu-id="cefc4-137">IIS launch profile</span></span>

<span data-ttu-id="cefc4-138">Créer un nouveau profil de lancement pour ajouter la prise en charge IIS au moment du développement :</span><span class="sxs-lookup"><span data-stu-id="cefc4-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="cefc4-139">Pour **profil**, sélectionnez le **nouveau** bouton.</span><span class="sxs-lookup"><span data-stu-id="cefc4-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="cefc4-140">Nommez le profil « IIS » dans la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="cefc4-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="cefc4-141">Sélectionnez **OK** pour créer le profil.</span><span class="sxs-lookup"><span data-stu-id="cefc4-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="cefc4-142">Pour le **lancer** paramètre, sélectionnez **IIS** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="cefc4-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="cefc4-143">Sélectionnez la case à cocher **lancement navigateur** et indiquez l’URL de point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="cefc4-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="cefc4-144">Utiliser le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cefc4-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="cefc4-145">Cet exemple utilise `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="cefc4-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="cefc4-146">Dans le **variables d’environnement** section, sélectionnez le **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="cefc4-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="cefc4-147">Fournir une variable d’environnement avec une clé de `ASPNETCORE_ENVIRONMENT` et la valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="cefc4-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="cefc4-148">Dans le **paramètres du serveur Web** zone, définissez la **URL de l’application**.</span><span class="sxs-lookup"><span data-stu-id="cefc4-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="cefc4-149">Cet exemple utilise `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="cefc4-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="cefc4-150">Enregistrer le profil.</span><span class="sxs-lookup"><span data-stu-id="cefc4-150">Save the profile.</span></span>

![Fenêtre de propriétés du projet avec l’onglet Débogage sélectionné.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="cefc4-155">Vous pouvez également ajouter manuellement un profil de lancement pour les [launchSettings.json](http://json.schemastore.org/launchsettings) fichier dans l’application :</span><span class="sxs-lookup"><span data-stu-id="cefc4-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="cefc4-156">Exécutez le projet</span><span class="sxs-lookup"><span data-stu-id="cefc4-156">Run the project</span></span>

<span data-ttu-id="cefc4-157">Dans l’interface utilisateur de Visual Studio, définissez le bouton exécuter sur le **IIS** de profil et sélectionnez le bouton pour démarrer l’application :</span><span class="sxs-lookup"><span data-stu-id="cefc4-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Bouton exécuter dans la barre d’outils de Visual Studio défini pour le profil « IIS ».](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="cefc4-159">Visual Studio peut demander un redémarrage si ne pas en cours d’exécution en tant qu’administrateur.</span><span class="sxs-lookup"><span data-stu-id="cefc4-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="cefc4-160">Si vous y êtes invité, redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cefc4-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="cefc4-161">Si un certificat non approuvé de développement est utilisé, le navigateur peut-être vous amener à créer une exception pour le certificat non approuvé.</span><span class="sxs-lookup"><span data-stu-id="cefc4-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cefc4-162">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cefc4-162">Additional resources</span></span>

* [<span data-ttu-id="cefc4-163">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="cefc4-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="cefc4-164">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cefc4-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="cefc4-165">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cefc4-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="cefc4-166">Appliquer HTTPS</span><span class="sxs-lookup"><span data-stu-id="cefc4-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
