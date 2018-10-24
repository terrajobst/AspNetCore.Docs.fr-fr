---
title: Utiliser plusieurs environnements dans ASP.NET Core
author: rick-anderson
description: Découvrez comment contrôler le comportement de l’application dans différents environnements dans les applications ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: de3c3fd5a2f0e49366d9d5b4e992d0247bcab0e5
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577520"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="0378d-103">Utiliser plusieurs environnements dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0378d-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="0378d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0378d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0378d-105">ASP.NET Core configure le comportement de l’application en fonction de l’environnement d’exécution à l’aide d’une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0378d-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="0378d-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0378d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="0378d-107">Environnements</span><span class="sxs-lookup"><span data-stu-id="0378d-107">Environments</span></span>

<span data-ttu-id="0378d-108">ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke la valeur dans [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="0378d-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="0378d-109">Vous pouvez affecter n’importe quelle valeur à `ASPNETCORE_ENVIRONMENT`, mais [trois valeurs](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) sont prises en charge par le framework : [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) et [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="0378d-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="0378d-110">Si `ASPNETCORE_ENVIRONMENT` n’est pas définie, la valeur par défaut est `Production`.</span><span class="sxs-lookup"><span data-stu-id="0378d-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="0378d-111">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="0378d-111">The preceding code:</span></span>

* <span data-ttu-id="0378d-112">Appelle [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) quand `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="0378d-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="0378d-113">Appelle [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quand `ASPNETCORE_ENVIRONMENT` est définie sur l’une des valeurs :</span><span class="sxs-lookup"><span data-stu-id="0378d-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="0378d-114">Le [Tag helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de la propriété `IHostingEnvironment.EnvironmentName` pour inclure ou exclure le balisage dans l’élément :</span><span class="sxs-lookup"><span data-stu-id="0378d-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="0378d-115">Sur Windows et macOS, les valeurs et les variables d’environnement ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="0378d-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="0378d-116">Les valeurs et les variables d’environnement Linux **respectent la casse** par défaut.</span><span class="sxs-lookup"><span data-stu-id="0378d-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="0378d-117">Développement</span><span class="sxs-lookup"><span data-stu-id="0378d-117">Development</span></span>

<span data-ttu-id="0378d-118">L’environnement de développement peut activer des fonctionnalités qui ne doivent pas être exposées en production.</span><span class="sxs-lookup"><span data-stu-id="0378d-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="0378d-119">Par exemple, les modèles ASP.NET Core activent la [page d’exception de développeur](xref:fundamentals/error-handling#the-developer-exception-page) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="0378d-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="0378d-120">L’environnement de développement de l’ordinateur local peut être défini dans le fichier *Properties\launchSettings.json* du projet.</span><span class="sxs-lookup"><span data-stu-id="0378d-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="0378d-121">Les valeurs d’environnement définies dans *launchSettings.json* remplacent les valeurs définies dans l’environnement système.</span><span class="sxs-lookup"><span data-stu-id="0378d-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="0378d-122">Le code JSON suivant montre trois profils à partir d’un fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="0378d-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="0378d-123">La propriété `applicationUrl` dans *launchSettings.json* peut spécifier une liste d’URL de serveur.</span><span class="sxs-lookup"><span data-stu-id="0378d-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="0378d-124">Utilisez un point-virgule entre les URL de la liste :</span><span class="sxs-lookup"><span data-stu-id="0378d-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="0378d-125">Quand l’application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run), le premier profil avec `"commandName": "Project"` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="0378d-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="0378d-126">La valeur de `commandName` spécifie le serveur web à lancer.</span><span class="sxs-lookup"><span data-stu-id="0378d-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="0378d-127">`commandName` peut avoir l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="0378d-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="0378d-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="0378d-128">IIS Express</span></span>
* <span data-ttu-id="0378d-129">IIS</span><span class="sxs-lookup"><span data-stu-id="0378d-129">IIS</span></span>
* <span data-ttu-id="0378d-130">Project (qui lance Kestrel)</span><span class="sxs-lookup"><span data-stu-id="0378d-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="0378d-131">Quand une application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="0378d-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="0378d-132">*launchSettings.json* est lu, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="0378d-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="0378d-133">Les paramètres `environmentVariables` dans *launchSettings.json* remplacent les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="0378d-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="0378d-134">L’environnement d’hébergement s’affiche.</span><span class="sxs-lookup"><span data-stu-id="0378d-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="0378d-135">La sortie suivante montre une application démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="0378d-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="0378d-136">L’onglet **Déboguer** des propriétés de projet Visual Studio fournit une interface graphique utilisateur qui permet de modifier le fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="0378d-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Propriétés de projet, définition des variables d’environnement](environments/_static/project-properties-debug.png)

<span data-ttu-id="0378d-138">Les modifications apportées aux profils de projet peuvent ne prendre effet qu’une fois le serveur web redémarré.</span><span class="sxs-lookup"><span data-stu-id="0378d-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="0378d-139">Vous devez redémarrer Kestrel pour qu’il puisse détecter les modifications apportées à son environnement.</span><span class="sxs-lookup"><span data-stu-id="0378d-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="0378d-140">*launchSettings.json* ne doit pas stocker de secrets.</span><span class="sxs-lookup"><span data-stu-id="0378d-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="0378d-141">Vous pouvez utiliser [l’outil Secret Manager](xref:security/app-secrets) afin de stocker des secrets pour le développement local.</span><span class="sxs-lookup"><span data-stu-id="0378d-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="0378d-142">Quand vous utilisez [Visual Studio Code](https://code.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="0378d-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="0378d-143">L’exemple suivant définit `Development` comme environnement :</span><span class="sxs-lookup"><span data-stu-id="0378d-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="0378d-144">Un fichier *.vscode/launch.json* dans le projet n’est pas lu au démarrage de l’application avec `dotnet run` de la même façon que *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0378d-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="0378d-145">Lors du lancement d’une application en cours de développement qui n’a pas de fichier *launchSettings.json*, vous devez définir l’environnement avec une variable d’environnement ou un argument de ligne de commande pour la commande `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="0378d-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="0378d-146">Production</span><span class="sxs-lookup"><span data-stu-id="0378d-146">Production</span></span>

<span data-ttu-id="0378d-147">Vous devez configurer l’environnement de production pour optimiser la sécurité, les performances et la robustesse de l’application.</span><span class="sxs-lookup"><span data-stu-id="0378d-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="0378d-148">Voici quelques paramètres courants qui diffèrent du développement :</span><span class="sxs-lookup"><span data-stu-id="0378d-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="0378d-149">La mise en cache.</span><span class="sxs-lookup"><span data-stu-id="0378d-149">Caching.</span></span>
* <span data-ttu-id="0378d-150">Les ressources côté client sont groupées, réduites et éventuellement servies à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="0378d-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="0378d-151">Les Pages d’erreur de diagnostic sont désactivées.</span><span class="sxs-lookup"><span data-stu-id="0378d-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="0378d-152">Les pages d’erreur conviviales sont activées.</span><span class="sxs-lookup"><span data-stu-id="0378d-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="0378d-153">La journalisation et la surveillance de la production sont activées.</span><span class="sxs-lookup"><span data-stu-id="0378d-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="0378d-154">Par exemple, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="0378d-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="0378d-155">Définir l’environnement</span><span class="sxs-lookup"><span data-stu-id="0378d-155">Set the environment</span></span>

<span data-ttu-id="0378d-156">Il est souvent utile de définir un environnement spécifique à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="0378d-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="0378d-157">Si l’environnement n’est pas défini, il prend par défaut la valeur `Production`, ce qui désactive la plupart des fonctionnalités de débogage.</span><span class="sxs-lookup"><span data-stu-id="0378d-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="0378d-158">La méthode de configuration de l’environnement dépend du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="0378d-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="0378d-159">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0378d-159">Azure App Service</span></span>

<span data-ttu-id="0378d-160">Pour définir l’environnement dans [Azure App Service](https://azure.microsoft.com/services/app-service/), effectuez les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0378d-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="0378d-161">Sélectionnez l’application dans le panneau **App Services**.</span><span class="sxs-lookup"><span data-stu-id="0378d-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="0378d-162">Dans le groupe **PARAMÈTRES**, sélectionnez le panneau **Paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="0378d-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="0378d-163">Dans la zone **Paramètres d’application**, sélectionnez **Ajouter un nouveau paramètre**.</span><span class="sxs-lookup"><span data-stu-id="0378d-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="0378d-164">Pour **Entrer un nom**, spécifiez `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0378d-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="0378d-165">Pour **Entrer une valeur**, spécifiez l’environnement (par exemple `Staging`).</span><span class="sxs-lookup"><span data-stu-id="0378d-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="0378d-166">Cochez la case **Paramètre d’emplacement** si vous souhaitez que le paramètre d’environnement reste avec l’emplacement actuel quand des emplacements de déploiement sont permutés.</span><span class="sxs-lookup"><span data-stu-id="0378d-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="0378d-167">Pour plus d’informations, consultez [Documentation Azure : Quels sont les paramètres qui sont permutés ?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="0378d-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="0378d-168">Sélectionnez **Enregistrer** en haut du panneau.</span><span class="sxs-lookup"><span data-stu-id="0378d-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="0378d-169">Azure App Service redémarre automatiquement l’application après qu’un paramètre d’application (variable d’environnement) est ajouté, changé ou supprimé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="0378d-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="0378d-170">Windows</span><span class="sxs-lookup"><span data-stu-id="0378d-170">Windows</span></span>

<span data-ttu-id="0378d-171">Pour définir `ASPNETCORE_ENVIRONMENT` pour la session actuelle quand l’application est démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run), les commandes suivantes sont utilisées :</span><span class="sxs-lookup"><span data-stu-id="0378d-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="0378d-172">**Invite de commandes**</span><span class="sxs-lookup"><span data-stu-id="0378d-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="0378d-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0378d-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="0378d-174">Ces commandes prennent effet uniquement pour la fenêtre active.</span><span class="sxs-lookup"><span data-stu-id="0378d-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="0378d-175">Quand la fenêtre est fermée, le paramètre `ASPNETCORE_ENVIRONMENT` reprend la valeur de machine ou la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="0378d-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="0378d-176">Pour définir la valeur globalement dans Windows, utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="0378d-176">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="0378d-177">Ouvrez le **Panneau de configuration** > **Système** > **Paramètres système avancés**, puis ajoutez ou modifiez la valeur `ASPNETCORE_ENVIRONMENT` :</span><span class="sxs-lookup"><span data-stu-id="0378d-177">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Propriétés système avancées](environments/_static/systemsetting_environment.png)

  ![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="0378d-180">Ouvrez une invite de commandes d’administration, puis utilisez la commande `setx`, ou ouvrez une invite de commandes PowerShell d’administration et utilisez `[Environment]::SetEnvironmentVariable` :</span><span class="sxs-lookup"><span data-stu-id="0378d-180">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="0378d-181">**Invite de commandes**</span><span class="sxs-lookup"><span data-stu-id="0378d-181">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="0378d-182">Le commutateur `/M` indique de définir la variable d’environnement au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="0378d-182">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="0378d-183">Si le commutateur `/M` n’est pas utilisé, la variable d’environnement est définie pour le compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0378d-183">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="0378d-184">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0378d-184">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="0378d-185">La valeur d’option `Machine` indique de définir la variable d’environnement au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="0378d-185">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="0378d-186">Si la valeur d’option est remplacée par `User`, la variable d’environnement est définie pour le compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0378d-186">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="0378d-187">Quand la variable d’environnement `ASPNETCORE_ENVIRONMENT` est définie globalement, elle prend effet pour `dotnet run` dans n’importe quelle fenêtre Commande ouverte une fois la valeur définie.</span><span class="sxs-lookup"><span data-stu-id="0378d-187">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="0378d-188">**web.config**</span><span class="sxs-lookup"><span data-stu-id="0378d-188">**web.config**</span></span>

<span data-ttu-id="0378d-189">Pour définir la variable d’environnement `ASPNETCORE_ENVIRONMENT` avec *web.config*, consultez la section *Définition des variables d’environnement* à l’adresse <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="0378d-189">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span> <span data-ttu-id="0378d-190">Quand la variable d’environnement `ASPNETCORE_ENVIRONMENT` est définie avec *web.config*, sa valeur remplace un paramètre au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="0378d-190">When the `ASPNETCORE_ENVIRONMENT` environment variable is set with *web.config*, its value overrides a setting at the system level.</span></span>

<span data-ttu-id="0378d-191">**Par pool d’applications IIS**</span><span class="sxs-lookup"><span data-stu-id="0378d-191">**Per IIS Application Pool**</span></span>

<span data-ttu-id="0378d-192">Pour définir la variables d’environnement `ASPNETCORE_ENVIRONMENT` pour une application qui s’exécute dans un pool d’applications isolé (prie en charge sur IIS 10.0 ou versions ultérieures), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="0378d-192">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="0378d-193">Quand la variable d’environnement `ASPNETCORE_ENVIRONMENT` est définie pour un pool d’applications, sa valeur remplace un paramètre au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="0378d-193">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0378d-194">Lors de l’hébergement d’une application dans IIS et de l’ajout ou du changement de la variable d’environnement `ASPNETCORE_ENVIRONMENT`, utilisez l’une des approches suivantes pour que la nouvelle valeur soit récupérée par des applications :</span><span class="sxs-lookup"><span data-stu-id="0378d-194">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="0378d-195">Exécutez la commande `net stop was /y` suivie de `net start w3svc` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="0378d-195">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="0378d-196">Redémarrez le serveur.</span><span class="sxs-lookup"><span data-stu-id="0378d-196">Restart the server.</span></span>

### <a name="macos"></a><span data-ttu-id="0378d-197">macOS</span><span class="sxs-lookup"><span data-stu-id="0378d-197">macOS</span></span>

<span data-ttu-id="0378d-198">Vous pouvez définir l’environnement actuel pour macOS en ligne durant l’exécution de l’application :</span><span class="sxs-lookup"><span data-stu-id="0378d-198">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="0378d-199">Vous pouvez également définir l’environnement avec `export` avant d’exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="0378d-199">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="0378d-200">Les variables d’environnement de niveau machine sont définies dans le fichier *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="0378d-200">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="0378d-201">Modifiez le fichier à l’aide d’un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="0378d-201">Edit the file using any text editor.</span></span> <span data-ttu-id="0378d-202">Ajoutez l’instruction suivante :</span><span class="sxs-lookup"><span data-stu-id="0378d-202">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="0378d-203">Linux</span><span class="sxs-lookup"><span data-stu-id="0378d-203">Linux</span></span>

<span data-ttu-id="0378d-204">Pour les versions Linux, exécutez la commande `export` à une invite de commandes pour les paramètres de variable basés sur la session, et le fichier *bash_profile* pour les paramètres d’environnement de niveau machine.</span><span class="sxs-lookup"><span data-stu-id="0378d-204">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="0378d-205">Configuration par environnement</span><span class="sxs-lookup"><span data-stu-id="0378d-205">Configuration by environment</span></span>

<span data-ttu-id="0378d-206">Pour charger la configuration par environnement, nous vous recommandons de disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="0378d-206">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="0378d-207">Fichiers *appSettings* (\*appsettings.&lt;<Environment>&gt;.json).</span><span class="sxs-lookup"><span data-stu-id="0378d-207">*appsettings* files (\*appsettings.&lt;<Environment>&gt;.json).</span></span> <span data-ttu-id="0378d-208">Consultez [Configuration : fournisseur de configuration de fichier](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0378d-208">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider).</span></span>
* <span data-ttu-id="0378d-209">Variables d’environnement (définies sur chaque système sur lequel l’application est hébergée).</span><span class="sxs-lookup"><span data-stu-id="0378d-209">environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="0378d-210">Voir [Configuration : fournisseur de configuration de fichier](xref:fundamentals/configuration/index#file-configuration-provider) et [Stockage sécurisé des secrets d’application lors du développement : variables d’environnement](xref:security/app-secrets#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="0378d-210">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider) and [Safe storage of app secrets in development: Environment variables](xref:security/app-secrets#environment-variables).</span></span>
* <span data-ttu-id="0378d-211">Secret Manager (dans l’environnement de développement uniquement).</span><span class="sxs-lookup"><span data-stu-id="0378d-211">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="0378d-212">Consultez <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="0378d-212">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="0378d-213">Classe et méthodes Startup en fonction de l’environnement</span><span class="sxs-lookup"><span data-stu-id="0378d-213">Environment-based Startup class and methods</span></span>

### <a name="startup-class-conventions"></a><span data-ttu-id="0378d-214">Conventions de la classe Startup</span><span class="sxs-lookup"><span data-stu-id="0378d-214">Startup class conventions</span></span>

<span data-ttu-id="0378d-215">Quand une application ASP.NET Core démarre, la [classe Startup](xref:fundamentals/startup) amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="0378d-215">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="0378d-216">L’application peut définir différentes classes `Startup` pour différents environnements (par exemple, `StartupDevelopment`), et la classe `Startup` appropriée est sélectionnée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0378d-216">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="0378d-217">La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="0378d-217">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="0378d-218">Si aucune classe `Startup{EnvironmentName}` correspondante n’est trouvée, la classe `Startup` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="0378d-218">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span>

<span data-ttu-id="0378d-219">Pour implémenter des classes `Startup` basées sur l’environnement, créez une classe `Startup{EnvironmentName}` pour chaque environnement en cours d’utilisation et une classe `Startup` de base :</span><span class="sxs-lookup"><span data-stu-id="0378d-219">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

<span data-ttu-id="0378d-220">Utilisez la surcharge [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) qui accepte un nom d’assembly :</span><span class="sxs-lookup"><span data-stu-id="0378d-220">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a><span data-ttu-id="0378d-221">Conventions de la méthode Startup</span><span class="sxs-lookup"><span data-stu-id="0378d-221">Startup method conventions</span></span>

<span data-ttu-id="0378d-222">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) prennent en charge les versions propres à l’environnement de la forme `Configure<EnvironmentName>` et `Configure<EnvironmentName>Services` :</span><span class="sxs-lookup"><span data-stu-id="0378d-222">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="0378d-223">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0378d-223">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="0378d-224">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="0378d-224">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
