---
title: Utilisation de plusieurs environnements dans ASP.NET Core
author: rick-anderson
description: "Découvrez comment ASP.NET Core fournit la prise en charge pour contrôler le comportement de l’application dans différents environnements."
keywords: "ASP.NET Core, les paramètres d’environnement, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="ca1b9-104">Utilisation de plusieurs environnements</span><span class="sxs-lookup"><span data-stu-id="ca1b9-104">Working with multiple environments</span></span>

<span data-ttu-id="ca1b9-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca1b9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ca1b9-106">ASP.NET Core prend en charge pour la définition du comportement de l’application lors de l’exécution avec les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-106">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="ca1b9-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ca1b9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="ca1b9-108">Environnements</span><span class="sxs-lookup"><span data-stu-id="ca1b9-108">Environments</span></span>

<span data-ttu-id="ca1b9-109">ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke cette valeur dans [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="ca1b9-109">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="ca1b9-110">`ASPNETCORE_ENVIRONMENT`peut être définie sur n’importe quelle valeur, mais [trois valeurs](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) sont pris en charge par l’infrastructure : [développement](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [intermédiaire](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), et [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="ca1b9-110">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="ca1b9-111">Si `ASPNETCORE_ENVIRONMENT` n’est pas défini, il prend par défaut `Production`.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-111">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="ca1b9-112">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-112">The preceding code:</span></span>

* <span data-ttu-id="ca1b9-113">Appels [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) lorsque `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-113">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="ca1b9-114">Appels [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) lorsque la valeur de `ASPNETCORE_ENVIRONMENT` est définie à une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-114">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="ca1b9-115">Le [assistance de balise d’environnement ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de `IHostingEnvironment.EnvironmentName` à inclure ou exclure le balisage dans l’élément :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-115">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="ca1b9-116">Remarque : Sur Windows et macOS, valeurs et variables d’environnement ne respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-116">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="ca1b9-117">Variables d’environnement Linux et les valeurs sont **casse** par défaut.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-117">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="ca1b9-118">Développement</span><span class="sxs-lookup"><span data-stu-id="ca1b9-118">Development</span></span>

<span data-ttu-id="ca1b9-119">L’environnement de développement peut activer les fonctionnalités qui ne doivent pas être exposées en production.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-119">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="ca1b9-120">Par exemple, les modèles ASP.NET Core activer la [page d’exception developer](xref:fundamentals/error-handling#the-developer-exception-page) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-120">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="ca1b9-121">L’environnement de développement de l’ordinateur local peut être définie dans le *Properties\launchSettings.json* fichier du projet.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-121">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="ca1b9-122">Définir des valeurs d’environnement dans *launchSettings.json* remplacent les valeurs définies dans l’environnement du système.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-122">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="ca1b9-123">Le code XML suivant montre les trois profils d’un *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-123">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="ca1b9-124">Lorsque l’application est lancée avec `dotnet run`, le premier profil avec `"commandName": "Project"` sera utilisé.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-124">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="ca1b9-125">La valeur de `commandName` Spécifie le serveur web pour lancer.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-125">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="ca1b9-126">`commandName`peut être :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-126">`commandName` can be one of :</span></span>

* <span data-ttu-id="ca1b9-127">IIS Express</span><span class="sxs-lookup"><span data-stu-id="ca1b9-127">IIS Express</span></span>
* <span data-ttu-id="ca1b9-128">IIS</span><span class="sxs-lookup"><span data-stu-id="ca1b9-128">IIS</span></span>
* <span data-ttu-id="ca1b9-129">Projet (ce qui lance le Kestrel)</span><span class="sxs-lookup"><span data-stu-id="ca1b9-129">Project (which launches Kestrel)</span></span>

<span data-ttu-id="ca1b9-130">Lorsque vous lancez une application avec `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="ca1b9-130">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="ca1b9-131">*launchSettings.json* est lu si elle est disponible.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-131">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="ca1b9-132">`environmentVariables`paramètres de *launchSettings.json* remplacer les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="ca1b9-133">L’environnement d’hébergement s’affiche.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-133">The hosting environment is displayed.</span></span>


<span data-ttu-id="ca1b9-134">La sortie suivante montre une application a démarré avec `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="ca1b9-134">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="ca1b9-135">Visual Studio **déboguer** onglet fournit une interface graphique utilisateur pour modifier la *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-135">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variables d’environnement de définition de propriétés de projet](environments/_static/project-properties-debug.png)

<span data-ttu-id="ca1b9-137">Modifications apportées aux profils de projet peuvent prendront effet qu’après le redémarrage du serveur web.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-137">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="ca1b9-138">Kestrel doit être redémarré avant qu’il détecte les modifications apportées à son environnement.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-138">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="ca1b9-139">*launchSettings.json* ne pas stocker les clés secrètes.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-139">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="ca1b9-140">Le [Secret gestionnaire](xref:security/app-secrets) peut être utilisé pour stocker des clés secrètes de développement local.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-140">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="ca1b9-141">Production</span><span class="sxs-lookup"><span data-stu-id="ca1b9-141">Production</span></span>

<span data-ttu-id="ca1b9-142">L’environnement de production doit être configuré pour optimiser la sécurité, performances et la robustesse de l’application.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-142">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="ca1b9-143">Certains paramètres courants susceptibles de présenter un environnement de production qui seraient diffèrent de développement incluent :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-143">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="ca1b9-144">La mise en cache.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-144">Caching.</span></span>
* <span data-ttu-id="ca1b9-145">Les ressources côté client sont fournies, réduites et potentiellement pris en charge à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-145">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="ca1b9-146">Pages d’erreur de diagnostic désactivés.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-146">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="ca1b9-147">Pages d’erreur convivial activées.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-147">Friendly error pages enabled.</span></span>
* <span data-ttu-id="ca1b9-148">Journalisation de production et surveillance activé.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-148">Production logging and monitoring enabled.</span></span> <span data-ttu-id="ca1b9-149">Par exemple, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="ca1b9-149">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="ca1b9-150">Configuration de l’environnement</span><span class="sxs-lookup"><span data-stu-id="ca1b9-150">Setting the environment</span></span>

<span data-ttu-id="ca1b9-151">Il est souvent utile de définir un environnement spécifique pour le test.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-151">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="ca1b9-152">Si l’environnement n’est pas défini, il prend par défaut `Production` qui désactive la plupart des fonctionnalités de débogage.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-152">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="ca1b9-153">La méthode de configuration de l’environnement dépend du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-153">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="ca1b9-154">Azure</span><span class="sxs-lookup"><span data-stu-id="ca1b9-154">Azure</span></span>

<span data-ttu-id="ca1b9-155">Pour le service d’application Azure :</span><span class="sxs-lookup"><span data-stu-id="ca1b9-155">For Azure app service:</span></span>

* <span data-ttu-id="ca1b9-156">Sélectionnez le **paramètres de l’Application** panneau.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-156">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="ca1b9-157">Ajouter la clé et la valeur de **paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-157">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="ca1b9-158">Windows</span><span class="sxs-lookup"><span data-stu-id="ca1b9-158">Windows</span></span>
<span data-ttu-id="ca1b9-159">Pour définir le `ASPNETCORE_ENVIRONMENT` pour la session active, si l’application est démarrée à l’aide de `dotnet run`, les commandes suivantes sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-159">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="ca1b9-160">**Ligne de commande**</span><span class="sxs-lookup"><span data-stu-id="ca1b9-160">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="ca1b9-161">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="ca1b9-161">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="ca1b9-162">Ces commandes prennent effet uniquement pour la fenêtre active.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-162">These commands take effect only for the current window.</span></span> <span data-ttu-id="ca1b9-163">Lorsque la fenêtre est fermée, le paramètre ASPNETCORE_ENVIRONMENT rétablit le paramètre par défaut ou la valeur de l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-163">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="ca1b9-164">Pour définir la valeur globalement à l’ouverture de Windows le **le panneau de configuration** > **système** > **les paramètres système avancés** et ajouter ou modifier le `ASPNETCORE_ENVIRONMENT` valeur.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-164">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Les propriétés avancées de système](environments/_static/systemsetting_environment.png)

![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="ca1b9-167">**web.config**</span><span class="sxs-lookup"><span data-stu-id="ca1b9-167">**web.config**</span></span>

<span data-ttu-id="ca1b9-168">Consultez le *définition des variables d’environnement* section de la [référence de configuration ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) rubrique.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-168">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="ca1b9-169">**Par Pool d’applications IIS**</span><span class="sxs-lookup"><span data-stu-id="ca1b9-169">**Per IIS Application Pool**</span></span>

<span data-ttu-id="ca1b9-170">Pour définir les variables d’environnement pour des applications qui s’exécutent dans des Pools d’applications isolées (pris en charge sur IIS 10.0 +), consultez le *commande AppCmd.exe* section de la [Variables d’environnement \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) rubrique.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-170">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="ca1b9-171">macOS</span><span class="sxs-lookup"><span data-stu-id="ca1b9-171">macOS</span></span>
<span data-ttu-id="ca1b9-172">Définition l’environnement actuel pour macOS peut être effectuée en ligne lors de l’exécution de l’application ;</span><span class="sxs-lookup"><span data-stu-id="ca1b9-172">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="ca1b9-173">ou à l’aide de `export` pour la définir avant d’exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-173">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="ca1b9-174">Variables d’environnement au niveau machine sont définies le *.bashrc* ou *.bash_profile* fichier.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-174">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="ca1b9-175">Modifiez le fichier à l’aide de n’importe quel éditeur de texte et ajoutez l’instruction suivante.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-175">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="ca1b9-176">Linux</span><span class="sxs-lookup"><span data-stu-id="ca1b9-176">Linux</span></span>
<span data-ttu-id="ca1b9-177">Pour les versions de Linux, utilisez le `export` commande à la ligne de commande pour la session en fonction des paramètres de variable et *bash_profile* fichier pour les paramètres d’environnement au niveau ordinateur.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-177">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="ca1b9-178">Configuration de l’environnement</span><span class="sxs-lookup"><span data-stu-id="ca1b9-178">Configuration by environment</span></span>

<span data-ttu-id="ca1b9-179">Consultez [Configuration par environnement](xref:fundamentals/configuration/index#configuration-by-environment) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-179">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="ca1b9-180">Environnement en fonction de méthodes et la classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-180">Environment based Startup class and methods</span></span>

<span data-ttu-id="ca1b9-181">Quand une application ASP.NET Core démarre, le [classe Startup](xref:fundamentals/startup) amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-181">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="ca1b9-182">Si une classe `Startup{EnvironmentName}` existe, que la classe sera appelée pour que `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="ca1b9-182">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="ca1b9-183">Remarque : L’appel [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) remplace les sections de configuration.</span><span class="sxs-lookup"><span data-stu-id="ca1b9-183">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="ca1b9-184">[Configurer](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) prennent en charge des versions spécifiques d’environnement du formulaire `Configure{EnvironmentName}` et `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="ca1b9-184">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="ca1b9-185">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ca1b9-185">Additional Resources</span></span>

* [<span data-ttu-id="ca1b9-186">Démarrage d’une application</span><span class="sxs-lookup"><span data-stu-id="ca1b9-186">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="ca1b9-187">Configuration</span><span class="sxs-lookup"><span data-stu-id="ca1b9-187">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="ca1b9-188">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="ca1b9-188">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
