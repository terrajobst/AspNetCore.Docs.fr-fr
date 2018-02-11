---
title: Utilisation de plusieurs environnements dans ASP.NET Core
author: rick-anderson
description: "Découvrez dans quelle mesure ASP.NET Core permet de contrôler le comportement d’une application entre différents environnements."
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b40ee9b1c6feae4942f05d22dab776d3cf6c26a0
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="0a4f3-103">Utilisation de plusieurs environnements</span><span class="sxs-lookup"><span data-stu-id="0a4f3-103">Working with multiple environments</span></span>

<span data-ttu-id="0a4f3-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a4f3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a4f3-105">ASP.NET Core prend en charge la définition du comportement d’une application au moment de l’exécution avec des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="0a4f3-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a4f3-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="0a4f3-107">Environnements</span><span class="sxs-lookup"><span data-stu-id="0a4f3-107">Environments</span></span>

<span data-ttu-id="0a4f3-108">ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke cette valeur dans [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="0a4f3-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="0a4f3-109">`ASPNETCORE_ENVIRONMENT` peut être définie sur n’importe quelle valeur, mais [trois valeurs](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) sont prises en charge par le framework : [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) et [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="0a4f3-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="0a4f3-110">Si `ASPNETCORE_ENVIRONMENT` n’est pas définie, elle prend par défaut la valeur `Production`.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="0a4f3-111">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-111">The preceding code:</span></span>

* <span data-ttu-id="0a4f3-112">Appelle [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quand `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="0a4f3-113">Appelle [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quand `ASPNETCORE_ENVIRONMENT` est définie sur l’une des valeurs :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="0a4f3-114">Le [Tag helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de la propriété `IHostingEnvironment.EnvironmentName` pour inclure ou exclure du balisage dans l’élément :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="0a4f3-115">Remarque : Sur Windows et macOS, les valeurs et les variables d’environnement ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="0a4f3-116">Les valeurs et les variables d’environnement Linux **respectent la casse** par défaut.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="0a4f3-117">Développement</span><span class="sxs-lookup"><span data-stu-id="0a4f3-117">Development</span></span>

<span data-ttu-id="0a4f3-118">L’environnement de développement peut activer des fonctionnalités qui ne doivent pas être exposées en production.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="0a4f3-119">Par exemple, les modèles ASP.NET Core activent la [page d’exception de développeur](xref:fundamentals/error-handling#the-developer-exception-page) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="0a4f3-120">L’environnement de développement de l’ordinateur local peut être défini dans le fichier *Properties\launchSettings.json* du projet.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="0a4f3-121">Les valeurs d’environnement définies dans *launchSettings.json* remplacent les valeurs définies dans l’environnement système.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="0a4f3-122">Le code JSON suivant montre trois profils à partir d’un fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="0a4f3-123">Quand l’application est lancée avec `dotnet run`, le premier profil avec `"commandName": "Project"` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="0a4f3-124">La valeur de `commandName` spécifie le serveur web à lancer.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="0a4f3-125">`commandName` peut avoir l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="0a4f3-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="0a4f3-126">IIS Express</span></span>
* <span data-ttu-id="0a4f3-127">IIS</span><span class="sxs-lookup"><span data-stu-id="0a4f3-127">IIS</span></span>
* <span data-ttu-id="0a4f3-128">Project (qui lance Kestrel)</span><span class="sxs-lookup"><span data-stu-id="0a4f3-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="0a4f3-129">Quand une application est lancée avec `dotnet run` :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="0a4f3-130">*launchSettings.json* est lu, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="0a4f3-131">Les paramètres `environmentVariables` dans *launchSettings.json* remplacent les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="0a4f3-132">L’environnement d’hébergement s’affiche.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="0a4f3-133">La sortie suivante montre une application démarrée avec `dotnet run` :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="0a4f3-134">L’onglet **Déboguer** de Visual Studio fournit une interface graphique utilisateur qui permet de modifier le fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Propriétés de projet, définition des variables d’environnement](environments/_static/project-properties-debug.png)

<span data-ttu-id="0a4f3-136">Les modifications apportées aux profils de projet peuvent ne prendre effet qu’une fois le serveur web redémarré.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="0a4f3-137">Kestrel doit être redémarré pour qu’il puisse détecter les modifications apportées à son environnement.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="0a4f3-138">*launchSettings.json* ne doit pas stocker de secrets.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="0a4f3-139">Vous pouvez utiliser [l’outil Secret Manager](xref:security/app-secrets) afin de stocker des secrets pour le développement local.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="0a4f3-140">Production</span><span class="sxs-lookup"><span data-stu-id="0a4f3-140">Production</span></span>

<span data-ttu-id="0a4f3-141">Vous devez configurer l’environnement de production pour optimiser la sécurité, les performances et la robustesse de l’application.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="0a4f3-142">Voici certains paramètres courants d’un environnement de production qui peuvent différer dans un environnement de développement :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="0a4f3-143">Mise en cache.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-143">Caching.</span></span>
* <span data-ttu-id="0a4f3-144">Les ressources côté client sont groupées, réduites et éventuellement servies à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="0a4f3-145">Les Pages d’erreur de diagnostic sont désactivées.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="0a4f3-146">Les pages d’erreur conviviales sont activées.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="0a4f3-147">La journalisation et la surveillance de la production sont activées.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="0a4f3-148">Par exemple, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="0a4f3-148">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="0a4f3-149">Définition de l’environnement</span><span class="sxs-lookup"><span data-stu-id="0a4f3-149">Setting the environment</span></span>

<span data-ttu-id="0a4f3-150">Il est souvent utile de définir un environnement spécifique à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="0a4f3-151">Si l’environnement n’est pas défini, il prend par défaut la valeur `Production` qui désactive la plupart des fonctionnalités de débogage.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="0a4f3-152">La méthode de configuration de l’environnement dépend du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="0a4f3-153">Azure</span><span class="sxs-lookup"><span data-stu-id="0a4f3-153">Azure</span></span>

<span data-ttu-id="0a4f3-154">Pour Azure App Service :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-154">For Azure app service:</span></span>

* <span data-ttu-id="0a4f3-155">Sélectionnez le panneau **Paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="0a4f3-156">Ajouter la clé et la valeur dans **Paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="0a4f3-157">Windows</span><span class="sxs-lookup"><span data-stu-id="0a4f3-157">Windows</span></span>
<span data-ttu-id="0a4f3-158">Pour définir `ASPNETCORE_ENVIRONMENT` pour la session actuelle, si l’application est démarrée à l’aide de `dotnet run`, les commandes suivantes sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="0a4f3-159">**Ligne de commande**</span><span class="sxs-lookup"><span data-stu-id="0a4f3-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="0a4f3-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0a4f3-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="0a4f3-161">Ces commandes prennent effet uniquement pour la fenêtre active.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="0a4f3-162">Quand la fenêtre est fermée, le paramètre ASPNETCORE_ENVIRONMENT rétablit la valeur de machine ou le paramètre par défaut.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="0a4f3-163">Pour définir la valeur globalement à l’ouverture de Windows, ouvrez le **Panneau de configuration** > **Système** > **Paramètres système avancés** et ajoutez ou modifiez la valeur `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propriétés système avancées](environments/_static/systemsetting_environment.png)

![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="0a4f3-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="0a4f3-166">**web.config**</span></span>

<span data-ttu-id="0a4f3-167">Consultez la section *Définition des variables d’environnement* de la rubrique [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="0a4f3-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="0a4f3-168">**Par pool d’applications IIS**</span><span class="sxs-lookup"><span data-stu-id="0a4f3-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="0a4f3-169">Pour définir des variables d’environnement pour des applications qui s’exécutent dans des pools d’applications isolés (prise en charge sur IIS 10.0+), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="0a4f3-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="0a4f3-170">macOS</span><span class="sxs-lookup"><span data-stu-id="0a4f3-170">macOS</span></span>
<span data-ttu-id="0a4f3-171">Vous pouvez définir l’environnement actuel pour macOS en ligne à l’exécution de l’application ;</span><span class="sxs-lookup"><span data-stu-id="0a4f3-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="0a4f3-172">ou vous pouvez utiliser `export` pour le définir avant d’exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="0a4f3-173">Les variables d’environnement de niveau machine sont définies dans le fichier *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="0a4f3-174">Modifiez le fichier à l’aide de n’importe quel éditeur de texte et ajoutez l’instruction suivante.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-174">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="0a4f3-175">Linux</span><span class="sxs-lookup"><span data-stu-id="0a4f3-175">Linux</span></span>
<span data-ttu-id="0a4f3-176">Pour les versions Linux, utilisez la commande `export` depuis la ligne de commande pour les paramètres de variable basés sur la session, et le fichier *bash_profile* pour les paramètres d’environnement de niveau machine.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="0a4f3-177">Configuration par environnement</span><span class="sxs-lookup"><span data-stu-id="0a4f3-177">Configuration by environment</span></span>

<span data-ttu-id="0a4f3-178">Pour plus d’informations, consultez [Configuration par environnement](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="0a4f3-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="0a4f3-179">Classe et méthodes Startup en fonction de l’environnement</span><span class="sxs-lookup"><span data-stu-id="0a4f3-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="0a4f3-180">Quand une application ASP.NET Core démarre, la [classe Startup](xref:fundamentals/startup) amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="0a4f3-181">Si une classe `Startup{EnvironmentName}` existe, elle est appelée pour cet `EnvironmentName` :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="0a4f3-182">Remarque : Appeler [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) remplace les sections de configuration.</span><span class="sxs-lookup"><span data-stu-id="0a4f3-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="0a4f3-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) prennent en charge des versions d’environnement spécifiques de la forme `Configure{EnvironmentName}` et `Configure{EnvironmentName}Services` :</span><span class="sxs-lookup"><span data-stu-id="0a4f3-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="0a4f3-184">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0a4f3-184">Additional resources</span></span>

* [<span data-ttu-id="0a4f3-185">Démarrage d’une application</span><span class="sxs-lookup"><span data-stu-id="0a4f3-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="0a4f3-186">Configuration</span><span class="sxs-lookup"><span data-stu-id="0a4f3-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="0a4f3-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="0a4f3-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
