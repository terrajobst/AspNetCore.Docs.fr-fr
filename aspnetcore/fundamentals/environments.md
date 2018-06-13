---
title: Utiliser plusieurs environnements dans ASP.NET Core
author: rick-anderson
description: Découvrez dans quelle mesure ASP.NET Core permet de contrôler le comportement d’une application entre différents environnements.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33840955"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="22890-103">Utiliser plusieurs environnements dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22890-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="22890-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="22890-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="22890-105">ASP.NET Core prend en charge la définition du comportement d’une application au moment de l’exécution avec des variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="22890-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="22890-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22890-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="22890-107">Environnements</span><span class="sxs-lookup"><span data-stu-id="22890-107">Environments</span></span>

<span data-ttu-id="22890-108">ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke cette valeur dans [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="22890-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="22890-109">`ASPNETCORE_ENVIRONMENT` peut être définie sur n’importe quelle valeur, mais [trois valeurs](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) sont prises en charge par le framework : [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) et [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="22890-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="22890-110">Si `ASPNETCORE_ENVIRONMENT` n’est pas définie, elle prend par défaut la valeur `Production`.</span><span class="sxs-lookup"><span data-stu-id="22890-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="22890-111">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="22890-111">The preceding code:</span></span>

* <span data-ttu-id="22890-112">Appelle [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quand `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="22890-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="22890-113">Appelle [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quand `ASPNETCORE_ENVIRONMENT` est définie sur l’une des valeurs :</span><span class="sxs-lookup"><span data-stu-id="22890-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="22890-114">Le [Tag helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de la propriété `IHostingEnvironment.EnvironmentName` pour inclure ou exclure du balisage dans l’élément :</span><span class="sxs-lookup"><span data-stu-id="22890-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="22890-115">Remarque : Sur Windows et macOS, les valeurs et les variables d’environnement ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="22890-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="22890-116">Les valeurs et les variables d’environnement Linux **respectent la casse** par défaut.</span><span class="sxs-lookup"><span data-stu-id="22890-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="22890-117">Développement</span><span class="sxs-lookup"><span data-stu-id="22890-117">Development</span></span>

<span data-ttu-id="22890-118">L’environnement de développement peut activer des fonctionnalités qui ne doivent pas être exposées en production.</span><span class="sxs-lookup"><span data-stu-id="22890-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="22890-119">Par exemple, les modèles ASP.NET Core activent la [page d’exception de développeur](xref:fundamentals/error-handling#the-developer-exception-page) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="22890-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="22890-120">L’environnement de développement de l’ordinateur local peut être défini dans le fichier *Properties\launchSettings.json* du projet.</span><span class="sxs-lookup"><span data-stu-id="22890-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="22890-121">Les valeurs d’environnement définies dans *launchSettings.json* remplacent les valeurs définies dans l’environnement système.</span><span class="sxs-lookup"><span data-stu-id="22890-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="22890-122">Le code JSON suivant montre trois profils à partir d’un fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="22890-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="22890-123">La propriété `applicationUrl` dans *launchSettings.json* peut spécifier une liste d’URL de serveur.</span><span class="sxs-lookup"><span data-stu-id="22890-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="22890-124">Utilisez un point-virgule entre les URL de la liste :</span><span class="sxs-lookup"><span data-stu-id="22890-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="22890-125">Quand l’application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run), le premier profil avec `"commandName": "Project"` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="22890-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="22890-126">La valeur de `commandName` spécifie le serveur web à lancer.</span><span class="sxs-lookup"><span data-stu-id="22890-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="22890-127">`commandName` peut avoir l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="22890-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="22890-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="22890-128">IIS Express</span></span>
* <span data-ttu-id="22890-129">IIS</span><span class="sxs-lookup"><span data-stu-id="22890-129">IIS</span></span>
* <span data-ttu-id="22890-130">Project (qui lance Kestrel)</span><span class="sxs-lookup"><span data-stu-id="22890-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="22890-131">Quand une application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="22890-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="22890-132">*launchSettings.json* est lu, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="22890-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="22890-133">Les paramètres `environmentVariables` dans *launchSettings.json* remplacent les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="22890-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="22890-134">L’environnement d’hébergement s’affiche.</span><span class="sxs-lookup"><span data-stu-id="22890-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="22890-135">La sortie suivante montre une application démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="22890-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="22890-136">L’onglet **Déboguer** de Visual Studio fournit une interface graphique utilisateur qui permet de modifier le fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="22890-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Propriétés de projet, définition des variables d’environnement](environments/_static/project-properties-debug.png)

<span data-ttu-id="22890-138">Les modifications apportées aux profils de projet peuvent ne prendre effet qu’une fois le serveur web redémarré.</span><span class="sxs-lookup"><span data-stu-id="22890-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="22890-139">Kestrel doit être redémarré pour qu’il puisse détecter les modifications apportées à son environnement.</span><span class="sxs-lookup"><span data-stu-id="22890-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="22890-140">*launchSettings.json* ne doit pas stocker de secrets.</span><span class="sxs-lookup"><span data-stu-id="22890-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="22890-141">Vous pouvez utiliser [l’outil Secret Manager](xref:security/app-secrets) afin de stocker des secrets pour le développement local.</span><span class="sxs-lookup"><span data-stu-id="22890-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="22890-142">Production</span><span class="sxs-lookup"><span data-stu-id="22890-142">Production</span></span>

<span data-ttu-id="22890-143">Vous devez configurer l’environnement de production pour optimiser la sécurité, les performances et la robustesse de l’application.</span><span class="sxs-lookup"><span data-stu-id="22890-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="22890-144">Voici quelques paramètres courants qui diffèrent du développement :</span><span class="sxs-lookup"><span data-stu-id="22890-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="22890-145">La mise en cache.</span><span class="sxs-lookup"><span data-stu-id="22890-145">Caching.</span></span>
* <span data-ttu-id="22890-146">Les ressources côté client sont groupées, réduites et éventuellement servies à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="22890-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="22890-147">Les Pages d’erreur de diagnostic sont désactivées.</span><span class="sxs-lookup"><span data-stu-id="22890-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="22890-148">Les pages d’erreur conviviales sont activées.</span><span class="sxs-lookup"><span data-stu-id="22890-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="22890-149">La journalisation et la surveillance de la production sont activées.</span><span class="sxs-lookup"><span data-stu-id="22890-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="22890-150">Par exemple, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="22890-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="22890-151">Définition de l’environnement</span><span class="sxs-lookup"><span data-stu-id="22890-151">Setting the environment</span></span>

<span data-ttu-id="22890-152">Il est souvent utile de définir un environnement spécifique à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="22890-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="22890-153">Si l’environnement n’est pas défini, il prend par défaut la valeur `Production` qui désactive la plupart des fonctionnalités de débogage.</span><span class="sxs-lookup"><span data-stu-id="22890-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="22890-154">La méthode de configuration de l’environnement dépend du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="22890-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="22890-155">Azure</span><span class="sxs-lookup"><span data-stu-id="22890-155">Azure</span></span>

<span data-ttu-id="22890-156">Pour Azure App Service :</span><span class="sxs-lookup"><span data-stu-id="22890-156">For Azure app service:</span></span>

* <span data-ttu-id="22890-157">Sélectionnez le panneau **Paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="22890-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="22890-158">Ajoutez la clé et la valeur dans **Paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="22890-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="22890-159">Windows</span><span class="sxs-lookup"><span data-stu-id="22890-159">Windows</span></span>
<span data-ttu-id="22890-160">Pour définir `ASPNETCORE_ENVIRONMENT` pour la session actuelle, si l’application est démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run), les commandes suivantes sont utilisées :</span><span class="sxs-lookup"><span data-stu-id="22890-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="22890-161">**Ligne de commande**</span><span class="sxs-lookup"><span data-stu-id="22890-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="22890-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="22890-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="22890-163">Ces commandes prennent effet uniquement pour la fenêtre active.</span><span class="sxs-lookup"><span data-stu-id="22890-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="22890-164">Quand la fenêtre est fermée, le paramètre ASPNETCORE_ENVIRONMENT rétablit la valeur de machine ou le paramètre par défaut.</span><span class="sxs-lookup"><span data-stu-id="22890-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="22890-165">Pour définir la valeur globalement à l’ouverture de Windows, ouvrez le **Panneau de configuration** > **Système** > **Paramètres système avancés** et ajoutez ou modifiez la valeur `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="22890-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propriétés système avancées](environments/_static/systemsetting_environment.png)

![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="22890-168">**web.config**</span><span class="sxs-lookup"><span data-stu-id="22890-168">**web.config**</span></span>

<span data-ttu-id="22890-169">Consultez la section *Définition des variables d’environnement* de la rubrique [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="22890-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="22890-170">**Par pool d’applications IIS**</span><span class="sxs-lookup"><span data-stu-id="22890-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="22890-171">Pour définir des variables d’environnement pour des applications qui s’exécutent dans des pools d’applications isolés (prise en charge sur IIS 10.0+), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="22890-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="22890-172">macOS</span><span class="sxs-lookup"><span data-stu-id="22890-172">macOS</span></span>
<span data-ttu-id="22890-173">Vous pouvez définir l’environnement actuel pour macOS en ligne à l’exécution de l’application ;</span><span class="sxs-lookup"><span data-stu-id="22890-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="22890-174">ou vous pouvez utiliser `export` pour le définir avant d’exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="22890-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="22890-175">Les variables d’environnement de niveau machine sont définies dans le fichier *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="22890-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="22890-176">Modifiez le fichier à l’aide de n’importe quel éditeur de texte et ajoutez l’instruction suivante.</span><span class="sxs-lookup"><span data-stu-id="22890-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="22890-177">Linux</span><span class="sxs-lookup"><span data-stu-id="22890-177">Linux</span></span>
<span data-ttu-id="22890-178">Pour les versions Linux, utilisez la commande `export` depuis la ligne de commande pour les paramètres de variable basés sur la session, et le fichier *bash_profile* pour les paramètres d’environnement de niveau machine.</span><span class="sxs-lookup"><span data-stu-id="22890-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="22890-179">Configuration par environnement</span><span class="sxs-lookup"><span data-stu-id="22890-179">Configuration by environment</span></span>

<span data-ttu-id="22890-180">Pour plus d’informations, consultez [Configuration par environnement](xref:fundamentals/configuration/index#configuration-by-environment).</span><span class="sxs-lookup"><span data-stu-id="22890-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="22890-181">Classe et méthodes Startup en fonction de l’environnement</span><span class="sxs-lookup"><span data-stu-id="22890-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="22890-182">Quand une application ASP.NET Core démarre, la [classe Startup](xref:fundamentals/startup) amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="22890-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="22890-183">Si une classe `Startup{EnvironmentName}` existe, elle est appelée pour cet `EnvironmentName` :</span><span class="sxs-lookup"><span data-stu-id="22890-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="22890-184">Remarque : Appeler [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) remplace les sections de configuration.</span><span class="sxs-lookup"><span data-stu-id="22890-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="22890-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) prennent en charge des versions d’environnement spécifiques de la forme `Configure{EnvironmentName}` et `Configure{EnvironmentName}Services` :</span><span class="sxs-lookup"><span data-stu-id="22890-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="22890-186">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="22890-186">Additional resources</span></span>

* [<span data-ttu-id="22890-187">Démarrage d’une application</span><span class="sxs-lookup"><span data-stu-id="22890-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="22890-188">Configuration</span><span class="sxs-lookup"><span data-stu-id="22890-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="22890-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="22890-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
