---
title: Utiliser plusieurs environnements dans ASP.NET Core
author: rick-anderson
description: Découvrez comment contrôler le comportement de l’application dans différents environnements dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: affbb95273c91fe5bf452e0e1ebefa669297304c
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944319"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="16ec8-103">Utiliser plusieurs environnements dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16ec8-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="16ec8-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16ec8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16ec8-105">ASP.NET Core configure le comportement de l’application en fonction de l’environnement d’exécution à l’aide d’une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="16ec8-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16ec8-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="16ec8-107">de développement</span><span class="sxs-lookup"><span data-stu-id="16ec8-107">Environments</span></span>

<span data-ttu-id="16ec8-108">ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke la valeur dans [IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="16ec8-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="16ec8-109">`ASPNETCORE_ENVIRONMENT` peut être définie sur n’importe quelle valeur, mais trois valeurs sont fournies par l’infrastructure :</span><span class="sxs-lookup"><span data-stu-id="16ec8-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="16ec8-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (par défaut)</span><span class="sxs-lookup"><span data-stu-id="16ec8-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="16ec8-111">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="16ec8-111">The preceding code:</span></span>

* <span data-ttu-id="16ec8-112">Appelle [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) quand `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="16ec8-113">Appelle [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quand `ASPNETCORE_ENVIRONMENT` est définie sur l’une des valeurs :</span><span class="sxs-lookup"><span data-stu-id="16ec8-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="16ec8-114">Le [Tag helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de la propriété `IHostingEnvironment.EnvironmentName` pour inclure ou exclure le balisage dans l’élément :</span><span class="sxs-lookup"><span data-stu-id="16ec8-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="16ec8-115">Sur Windows et macOS, les valeurs et les variables d’environnement ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="16ec8-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="16ec8-116">Les valeurs et les variables d’environnement Linux **respectent la casse** par défaut.</span><span class="sxs-lookup"><span data-stu-id="16ec8-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="16ec8-117">Développement</span><span class="sxs-lookup"><span data-stu-id="16ec8-117">Development</span></span>

<span data-ttu-id="16ec8-118">L’environnement de développement peut activer des fonctionnalités qui ne doivent pas être exposées en production.</span><span class="sxs-lookup"><span data-stu-id="16ec8-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="16ec8-119">Par exemple, les modèles ASP.NET Core activent la [page d’exceptions du développeur](xref:fundamentals/error-handling#developer-exception-page) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="16ec8-120">L’environnement de développement de l’ordinateur local peut être défini dans le fichier *Properties\launchSettings.json* du projet.</span><span class="sxs-lookup"><span data-stu-id="16ec8-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="16ec8-121">Les valeurs d’environnement définies dans *launchSettings.json* remplacent les valeurs définies dans l’environnement système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="16ec8-122">Le code JSON suivant montre trois profils à partir d’un fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="16ec8-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="16ec8-123">La propriété `applicationUrl` dans *launchSettings.json* peut spécifier une liste d’URL de serveur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="16ec8-124">Utilisez un point-virgule entre les URL de la liste :</span><span class="sxs-lookup"><span data-stu-id="16ec8-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="16ec8-125">Quand l’application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run), le premier profil avec `"commandName": "Project"` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="16ec8-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="16ec8-126">La valeur de `commandName` spécifie le serveur web à lancer.</span><span class="sxs-lookup"><span data-stu-id="16ec8-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="16ec8-127">`commandName` peut avoir l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="16ec8-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="16ec8-128">`Project` (qui lance Kestrel)</span><span class="sxs-lookup"><span data-stu-id="16ec8-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="16ec8-129">Quand une application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="16ec8-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="16ec8-130">*launchSettings.json* est lu, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="16ec8-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="16ec8-131">Les paramètres `environmentVariables` dans *launchSettings.json* remplacent les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="16ec8-132">L’environnement d’hébergement s’affiche.</span><span class="sxs-lookup"><span data-stu-id="16ec8-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="16ec8-133">La sortie suivante montre une application démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="16ec8-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="16ec8-134">L’onglet **Déboguer** des propriétés de projet Visual Studio fournit une interface graphique utilisateur qui permet de modifier le fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="16ec8-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Propriétés de projet, définition des variables d’environnement](environments/_static/project-properties-debug.png)

<span data-ttu-id="16ec8-136">Les modifications apportées aux profils de projet peuvent ne prendre effet qu’une fois le serveur web redémarré.</span><span class="sxs-lookup"><span data-stu-id="16ec8-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="16ec8-137">Vous devez redémarrer Kestrel pour qu’il puisse détecter les modifications apportées à son environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="16ec8-138">*launchSettings.json* ne doit pas stocker de secrets.</span><span class="sxs-lookup"><span data-stu-id="16ec8-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="16ec8-139">Vous pouvez utiliser [l’outil Secret Manager](xref:security/app-secrets) afin de stocker des secrets pour le développement local.</span><span class="sxs-lookup"><span data-stu-id="16ec8-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="16ec8-140">Quand vous utilisez [Visual Studio Code](https://code.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="16ec8-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="16ec8-141">L’exemple suivant définit `Development` comme environnement :</span><span class="sxs-lookup"><span data-stu-id="16ec8-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="16ec8-142">Un fichier *.vscode/launch.json* dans le projet n’est pas lu au démarrage de l’application avec `dotnet run` de la même façon que *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="16ec8-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="16ec8-143">Lors du lancement d’une application en cours de développement qui n’a pas de fichier *launchSettings.json*, vous devez définir l’environnement avec une variable d’environnement ou un argument de ligne de commande pour la commande `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="16ec8-144">Production</span><span class="sxs-lookup"><span data-stu-id="16ec8-144">Production</span></span>

<span data-ttu-id="16ec8-145">Vous devez configurer l’environnement de production pour optimiser la sécurité, les performances et la robustesse de l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="16ec8-146">Voici quelques paramètres courants qui diffèrent du développement :</span><span class="sxs-lookup"><span data-stu-id="16ec8-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="16ec8-147">La mise en cache.</span><span class="sxs-lookup"><span data-stu-id="16ec8-147">Caching.</span></span>
* <span data-ttu-id="16ec8-148">Les ressources côté client sont groupées, réduites et éventuellement servies à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="16ec8-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="16ec8-149">Les Pages d’erreur de diagnostic sont désactivées.</span><span class="sxs-lookup"><span data-stu-id="16ec8-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="16ec8-150">Les pages d’erreur conviviales sont activées.</span><span class="sxs-lookup"><span data-stu-id="16ec8-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="16ec8-151">La journalisation et la surveillance de la production sont activées.</span><span class="sxs-lookup"><span data-stu-id="16ec8-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="16ec8-152">Par exemple, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="16ec8-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="16ec8-153">Définir l’environnement</span><span class="sxs-lookup"><span data-stu-id="16ec8-153">Set the environment</span></span>

<span data-ttu-id="16ec8-154">Il est souvent utile de définir un environnement spécifique pour le test avec une variable d’environnement ou un paramètre de plateforme.</span><span class="sxs-lookup"><span data-stu-id="16ec8-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="16ec8-155">Si l’environnement n’est pas défini, il prend par défaut la valeur `Production`, ce qui désactive la plupart des fonctionnalités de débogage.</span><span class="sxs-lookup"><span data-stu-id="16ec8-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="16ec8-156">La méthode de configuration de l’environnement dépend du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="16ec8-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="16ec8-157">Lorsque l’hôte est généré, le dernier paramètre d’environnement lu par l’application détermine l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="16ec8-158">L’environnement de l’application ne peut pas être modifié tant que l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="16ec8-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="16ec8-159">Variable d’environnement ou paramètre de plateforme</span><span class="sxs-lookup"><span data-stu-id="16ec8-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="16ec8-160">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="16ec8-160">Azure App Service</span></span>

<span data-ttu-id="16ec8-161">Pour définir l’environnement dans [Azure App Service](https://azure.microsoft.com/services/app-service/), effectuez les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="16ec8-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="16ec8-162">Sélectionnez l’application dans le panneau **App Services**.</span><span class="sxs-lookup"><span data-stu-id="16ec8-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="16ec8-163">Dans le groupe **PARAMÈTRES**, sélectionnez le panneau **Paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="16ec8-163">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="16ec8-164">Dans la zone **Paramètres d’application**, sélectionnez **Ajouter un nouveau paramètre**.</span><span class="sxs-lookup"><span data-stu-id="16ec8-164">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="16ec8-165">Pour **Entrer un nom**, spécifiez `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-165">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="16ec8-166">Pour **Entrer une valeur**, spécifiez l’environnement (par exemple `Staging`).</span><span class="sxs-lookup"><span data-stu-id="16ec8-166">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="16ec8-167">Cochez la case **Paramètre d’emplacement** si vous souhaitez que le paramètre d’environnement reste avec l’emplacement actuel quand des emplacements de déploiement sont permutés.</span><span class="sxs-lookup"><span data-stu-id="16ec8-167">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="16ec8-168">Pour plus d’informations, consultez [Documentation Azure : Quels sont les paramètres qui sont permutés ?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="16ec8-168">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="16ec8-169">Sélectionnez **Enregistrer** en haut du panneau.</span><span class="sxs-lookup"><span data-stu-id="16ec8-169">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="16ec8-170">Azure App Service redémarre automatiquement l’application après qu’un paramètre d’application (variable d’environnement) est ajouté, changé ou supprimé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="16ec8-170">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="16ec8-171">Portail</span><span class="sxs-lookup"><span data-stu-id="16ec8-171">Windows</span></span>

<span data-ttu-id="16ec8-172">Pour définir `ASPNETCORE_ENVIRONMENT` pour la session actuelle quand l’application est démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run), les commandes suivantes sont utilisées :</span><span class="sxs-lookup"><span data-stu-id="16ec8-172">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="16ec8-173">**Invite de commandes**</span><span class="sxs-lookup"><span data-stu-id="16ec8-173">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="16ec8-174">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="16ec8-174">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="16ec8-175">Ces commandes prennent effet uniquement pour la fenêtre active.</span><span class="sxs-lookup"><span data-stu-id="16ec8-175">These commands only take effect for the current window.</span></span> <span data-ttu-id="16ec8-176">Quand la fenêtre est fermée, le paramètre `ASPNETCORE_ENVIRONMENT` reprend la valeur de machine ou la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="16ec8-176">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="16ec8-177">Pour définir la valeur globalement dans Windows, utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="16ec8-177">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="16ec8-178">Ouvrez le **panneau** de configuration > **système** > **paramètres système avancés** et ajoutez ou modifiez la valeur de `ASPNETCORE_ENVIRONMENT` :</span><span class="sxs-lookup"><span data-stu-id="16ec8-178">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Propriétés système avancées](environments/_static/systemsetting_environment.png)

  ![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="16ec8-181">Ouvrez une invite de commandes d’administration, puis utilisez la commande `setx`, ou ouvrez une invite de commandes PowerShell d’administration et utilisez `[Environment]::SetEnvironmentVariable` :</span><span class="sxs-lookup"><span data-stu-id="16ec8-181">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="16ec8-182">**Invite de commandes**</span><span class="sxs-lookup"><span data-stu-id="16ec8-182">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="16ec8-183">Le commutateur `/M` indique de définir la variable d’environnement au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-183">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="16ec8-184">Si le commutateur `/M` n’est pas utilisé, la variable d’environnement est définie pour le compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-184">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="16ec8-185">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="16ec8-185">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="16ec8-186">La valeur d’option `Machine` indique de définir la variable d’environnement au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-186">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="16ec8-187">Si la valeur d’option est remplacée par `User`, la variable d’environnement est définie pour le compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-187">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="16ec8-188">Quand la variable d’environnement `ASPNETCORE_ENVIRONMENT` est définie globalement, elle prend effet pour `dotnet run` dans n’importe quelle fenêtre Commande ouverte une fois la valeur définie.</span><span class="sxs-lookup"><span data-stu-id="16ec8-188">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="16ec8-189">**web.config**</span><span class="sxs-lookup"><span data-stu-id="16ec8-189">**web.config**</span></span>

<span data-ttu-id="16ec8-190">Pour définir la variable d’environnement `ASPNETCORE_ENVIRONMENT` avec *web.config*, consultez la section *Définition des variables d’environnement* à l’adresse <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-190">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="16ec8-191">**Fichier projet ou profil de publication**</span><span class="sxs-lookup"><span data-stu-id="16ec8-191">**Project file or publish profile**</span></span>

<span data-ttu-id="16ec8-192">**Pour les déploiements de Windows IIS :** Incluez la propriété `<EnvironmentName>` dans le [profil de publication (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) ou le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="16ec8-192">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="16ec8-193">Cette approche définit l’environnement dans *web.config* lorsque le projet est publié :</span><span class="sxs-lookup"><span data-stu-id="16ec8-193">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="16ec8-194">**Par pool d’applications IIS**</span><span class="sxs-lookup"><span data-stu-id="16ec8-194">**Per IIS Application Pool**</span></span>

<span data-ttu-id="16ec8-195">Pour définir la variables d’environnement `ASPNETCORE_ENVIRONMENT` pour une application qui s’exécute dans un pool d’applications isolé (prie en charge sur IIS 10.0 ou versions ultérieures), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="16ec8-195">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="16ec8-196">Quand la variable d’environnement `ASPNETCORE_ENVIRONMENT` est définie pour un pool d’applications, sa valeur remplace un paramètre au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-196">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16ec8-197">Lors de l’hébergement d’une application dans IIS et de l’ajout ou du changement de la variable d’environnement `ASPNETCORE_ENVIRONMENT`, utilisez l’une des approches suivantes pour que la nouvelle valeur soit récupérée par des applications :</span><span class="sxs-lookup"><span data-stu-id="16ec8-197">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="16ec8-198">Exécutez la commande `net stop was /y` suivie de `net start w3svc` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="16ec8-198">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="16ec8-199">Redémarrez le serveur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-199">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="16ec8-200">macOS</span><span class="sxs-lookup"><span data-stu-id="16ec8-200">macOS</span></span>

<span data-ttu-id="16ec8-201">Vous pouvez définir l’environnement actuel pour macOS en ligne durant l’exécution de l’application :</span><span class="sxs-lookup"><span data-stu-id="16ec8-201">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="16ec8-202">Vous pouvez également définir l’environnement avec `export` avant d’exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="16ec8-202">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="16ec8-203">Les variables d’environnement de niveau machine sont définies dans le fichier *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="16ec8-203">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="16ec8-204">Modifiez le fichier à l’aide d’un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="16ec8-204">Edit the file using any text editor.</span></span> <span data-ttu-id="16ec8-205">Ajoutez l’instruction suivante :</span><span class="sxs-lookup"><span data-stu-id="16ec8-205">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="16ec8-206">Linux</span><span class="sxs-lookup"><span data-stu-id="16ec8-206">Linux</span></span>

<span data-ttu-id="16ec8-207">Pour les versions Linux, exécutez la commande `export` à une invite de commandes pour les paramètres de variable basés sur la session, et le fichier *bash_profile* pour les paramètres d’environnement de niveau machine.</span><span class="sxs-lookup"><span data-stu-id="16ec8-207">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="16ec8-208">Définir l’environnement dans le code</span><span class="sxs-lookup"><span data-stu-id="16ec8-208">Set the environment in code</span></span>

<span data-ttu-id="16ec8-209">Appelez <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> lors de la génération de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="16ec8-209">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="16ec8-210">Consultez <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-210">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="16ec8-211">Configuration par environnement</span><span class="sxs-lookup"><span data-stu-id="16ec8-211">Configuration by environment</span></span>

<span data-ttu-id="16ec8-212">Pour charger la configuration par environnement, nous vous recommandons de disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="16ec8-212">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="16ec8-213">fichiers *appSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="16ec8-213">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="16ec8-214">Consultez <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-214">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="16ec8-215">Variables d’environnement (définies sur chaque système sur lequel l’application est hébergée).</span><span class="sxs-lookup"><span data-stu-id="16ec8-215">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="16ec8-216">Consultez <xref:fundamentals/host/generic-host#environmentname> et <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-216">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="16ec8-217">Secret Manager (dans l’environnement de développement uniquement).</span><span class="sxs-lookup"><span data-stu-id="16ec8-217">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="16ec8-218">Consultez <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-218">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="16ec8-219">Classe et méthodes Startup en fonction de l’environnement</span><span class="sxs-lookup"><span data-stu-id="16ec8-219">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="16ec8-220">Injectez IWebHostEnvironment dans Startup. configure</span><span class="sxs-lookup"><span data-stu-id="16ec8-220">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="16ec8-221">Injectez <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-221">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="16ec8-222">Cette approche est utile lorsque l’application doit uniquement ajuster `Startup.Configure` pour quelques environnements avec des différences de code minimales par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-222">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="16ec8-223">Injecter IWebHostEnvironment dans la classe Startup</span><span class="sxs-lookup"><span data-stu-id="16ec8-223">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="16ec8-224">Injectez <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> dans le constructeur `Startup`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-224">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="16ec8-225">Cette approche est utile lorsque l’application requiert la configuration de `Startup` pour quelques environnements avec des différences de code minimales par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-225">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="16ec8-226">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="16ec8-226">In the following example:</span></span>

* <span data-ttu-id="16ec8-227">L’environnement est conservé dans le champ `_env`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-227">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="16ec8-228">`_env` est utilisé dans `ConfigureServices` et `Configure` pour appliquer la configuration de démarrage en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-228">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```
### <a name="startup-class-conventions"></a><span data-ttu-id="16ec8-229">Conventions de la classe Startup</span><span class="sxs-lookup"><span data-stu-id="16ec8-229">Startup class conventions</span></span>

<span data-ttu-id="16ec8-230">Quand une application ASP.NET Core démarre, la [classe Startup](xref:fundamentals/startup) amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-230">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="16ec8-231">L’application peut définir des classes de `Startup` distinctes pour différents environnements (par exemple, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="16ec8-231">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="16ec8-232">La classe de `Startup` appropriée est sélectionnée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="16ec8-232">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="16ec8-233">La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="16ec8-233">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="16ec8-234">Si aucune classe `Startup{EnvironmentName}` correspondante n’est trouvée, la classe `Startup` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="16ec8-234">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="16ec8-235">Cette approche est utile lorsque l’application requiert la configuration du démarrage pour plusieurs environnements avec de nombreuses différences de code par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-235">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="16ec8-236">Pour implémenter des classes `Startup` basées sur l’environnement, créez une classe `Startup{EnvironmentName}` pour chaque environnement en cours d’utilisation et une classe `Startup` de base :</span><span class="sxs-lookup"><span data-stu-id="16ec8-236">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="16ec8-237">Utilisez la surcharge [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) qui accepte un nom d’assembly :</span><span class="sxs-lookup"><span data-stu-id="16ec8-237">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="16ec8-238">Conventions de la méthode Startup</span><span class="sxs-lookup"><span data-stu-id="16ec8-238">Startup method conventions</span></span>

<span data-ttu-id="16ec8-239">[Configurez](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) prennent en charge des versions spécifiques à l’environnement de la forme `Configure<EnvironmentName>` et `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-239">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="16ec8-240">Cette approche est utile lorsque l’application requiert la configuration du démarrage pour plusieurs environnements avec de nombreuses différences de code par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-240">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="16ec8-241">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="16ec8-241">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="16ec8-242">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16ec8-242">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16ec8-243">ASP.NET Core configure le comportement de l’application en fonction de l’environnement d’exécution à l’aide d’une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-243">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="16ec8-244">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16ec8-244">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="16ec8-245">de développement</span><span class="sxs-lookup"><span data-stu-id="16ec8-245">Environments</span></span>

<span data-ttu-id="16ec8-246">ASP.NET Core lit la variable d’environnement `ASPNETCORE_ENVIRONMENT` au démarrage de l’application et stocke la valeur dans [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="16ec8-246">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="16ec8-247">`ASPNETCORE_ENVIRONMENT` peut être définie sur n’importe quelle valeur, mais trois valeurs sont fournies par l’infrastructure :</span><span class="sxs-lookup"><span data-stu-id="16ec8-247">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="16ec8-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (par défaut)</span><span class="sxs-lookup"><span data-stu-id="16ec8-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="16ec8-249">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="16ec8-249">The preceding code:</span></span>

* <span data-ttu-id="16ec8-250">Appelle [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) quand `ASPNETCORE_ENVIRONMENT` a la valeur `Development`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-250">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="16ec8-251">Appelle [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quand `ASPNETCORE_ENVIRONMENT` est définie sur l’une des valeurs :</span><span class="sxs-lookup"><span data-stu-id="16ec8-251">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="16ec8-252">Le [Tag helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilise la valeur de la propriété `IHostingEnvironment.EnvironmentName` pour inclure ou exclure le balisage dans l’élément :</span><span class="sxs-lookup"><span data-stu-id="16ec8-252">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="16ec8-253">Sur Windows et macOS, les valeurs et les variables d’environnement ne respectent pas la casse.</span><span class="sxs-lookup"><span data-stu-id="16ec8-253">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="16ec8-254">Les valeurs et les variables d’environnement Linux **respectent la casse** par défaut.</span><span class="sxs-lookup"><span data-stu-id="16ec8-254">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="16ec8-255">Développement</span><span class="sxs-lookup"><span data-stu-id="16ec8-255">Development</span></span>

<span data-ttu-id="16ec8-256">L’environnement de développement peut activer des fonctionnalités qui ne doivent pas être exposées en production.</span><span class="sxs-lookup"><span data-stu-id="16ec8-256">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="16ec8-257">Par exemple, les modèles ASP.NET Core activent la [page d’exceptions du développeur](xref:fundamentals/error-handling#developer-exception-page) dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-257">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="16ec8-258">L’environnement de développement de l’ordinateur local peut être défini dans le fichier *Properties\launchSettings.json* du projet.</span><span class="sxs-lookup"><span data-stu-id="16ec8-258">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="16ec8-259">Les valeurs d’environnement définies dans *launchSettings.json* remplacent les valeurs définies dans l’environnement système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-259">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="16ec8-260">Le code JSON suivant montre trois profils à partir d’un fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="16ec8-260">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

> [!NOTE]
> <span data-ttu-id="16ec8-261">La propriété `applicationUrl` dans *launchSettings.json* peut spécifier une liste d’URL de serveur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-261">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="16ec8-262">Utilisez un point-virgule entre les URL de la liste :</span><span class="sxs-lookup"><span data-stu-id="16ec8-262">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="16ec8-263">Quand l’application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run), le premier profil avec `"commandName": "Project"` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="16ec8-263">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="16ec8-264">La valeur de `commandName` spécifie le serveur web à lancer.</span><span class="sxs-lookup"><span data-stu-id="16ec8-264">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="16ec8-265">`commandName` peut avoir l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="16ec8-265">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="16ec8-266">`Project` (qui lance Kestrel)</span><span class="sxs-lookup"><span data-stu-id="16ec8-266">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="16ec8-267">Quand une application est lancée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="16ec8-267">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="16ec8-268">*launchSettings.json* est lu, s’il est disponible.</span><span class="sxs-lookup"><span data-stu-id="16ec8-268">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="16ec8-269">Les paramètres `environmentVariables` dans *launchSettings.json* remplacent les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-269">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="16ec8-270">L’environnement d’hébergement s’affiche.</span><span class="sxs-lookup"><span data-stu-id="16ec8-270">The hosting environment is displayed.</span></span>

<span data-ttu-id="16ec8-271">La sortie suivante montre une application démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run) :</span><span class="sxs-lookup"><span data-stu-id="16ec8-271">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="16ec8-272">L’onglet **Déboguer** des propriétés de projet Visual Studio fournit une interface graphique utilisateur qui permet de modifier le fichier *launchSettings.json* :</span><span class="sxs-lookup"><span data-stu-id="16ec8-272">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Propriétés de projet, définition des variables d’environnement](environments/_static/project-properties-debug.png)

<span data-ttu-id="16ec8-274">Les modifications apportées aux profils de projet peuvent ne prendre effet qu’une fois le serveur web redémarré.</span><span class="sxs-lookup"><span data-stu-id="16ec8-274">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="16ec8-275">Vous devez redémarrer Kestrel pour qu’il puisse détecter les modifications apportées à son environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-275">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="16ec8-276">*launchSettings.json* ne doit pas stocker de secrets.</span><span class="sxs-lookup"><span data-stu-id="16ec8-276">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="16ec8-277">Vous pouvez utiliser [l’outil Secret Manager](xref:security/app-secrets) afin de stocker des secrets pour le développement local.</span><span class="sxs-lookup"><span data-stu-id="16ec8-277">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="16ec8-278">Quand vous utilisez [Visual Studio Code](https://code.visualstudio.com/), les variables d’environnement peuvent être définies dans le fichier *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="16ec8-278">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="16ec8-279">L’exemple suivant définit `Development` comme environnement :</span><span class="sxs-lookup"><span data-stu-id="16ec8-279">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="16ec8-280">Un fichier *.vscode/launch.json* dans le projet n’est pas lu au démarrage de l’application avec `dotnet run` de la même façon que *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="16ec8-280">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="16ec8-281">Lors du lancement d’une application en cours de développement qui n’a pas de fichier *launchSettings.json*, vous devez définir l’environnement avec une variable d’environnement ou un argument de ligne de commande pour la commande `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-281">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="16ec8-282">Production</span><span class="sxs-lookup"><span data-stu-id="16ec8-282">Production</span></span>

<span data-ttu-id="16ec8-283">Vous devez configurer l’environnement de production pour optimiser la sécurité, les performances et la robustesse de l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-283">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="16ec8-284">Voici quelques paramètres courants qui diffèrent du développement :</span><span class="sxs-lookup"><span data-stu-id="16ec8-284">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="16ec8-285">La mise en cache.</span><span class="sxs-lookup"><span data-stu-id="16ec8-285">Caching.</span></span>
* <span data-ttu-id="16ec8-286">Les ressources côté client sont groupées, réduites et éventuellement servies à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="16ec8-286">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="16ec8-287">Les Pages d’erreur de diagnostic sont désactivées.</span><span class="sxs-lookup"><span data-stu-id="16ec8-287">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="16ec8-288">Les pages d’erreur conviviales sont activées.</span><span class="sxs-lookup"><span data-stu-id="16ec8-288">Friendly error pages enabled.</span></span>
* <span data-ttu-id="16ec8-289">La journalisation et la surveillance de la production sont activées.</span><span class="sxs-lookup"><span data-stu-id="16ec8-289">Production logging and monitoring enabled.</span></span> <span data-ttu-id="16ec8-290">Par exemple, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="16ec8-290">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="16ec8-291">Définir l’environnement</span><span class="sxs-lookup"><span data-stu-id="16ec8-291">Set the environment</span></span>

<span data-ttu-id="16ec8-292">Il est souvent utile de définir un environnement spécifique pour le test avec une variable d’environnement ou un paramètre de plateforme.</span><span class="sxs-lookup"><span data-stu-id="16ec8-292">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="16ec8-293">Si l’environnement n’est pas défini, il prend par défaut la valeur `Production`, ce qui désactive la plupart des fonctionnalités de débogage.</span><span class="sxs-lookup"><span data-stu-id="16ec8-293">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="16ec8-294">La méthode de configuration de l’environnement dépend du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="16ec8-294">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="16ec8-295">Lorsque l’hôte est généré, le dernier paramètre d’environnement lu par l’application détermine l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-295">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="16ec8-296">L’environnement de l’application ne peut pas être modifié tant que l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="16ec8-296">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="16ec8-297">Variable d’environnement ou paramètre de plateforme</span><span class="sxs-lookup"><span data-stu-id="16ec8-297">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="16ec8-298">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="16ec8-298">Azure App Service</span></span>

<span data-ttu-id="16ec8-299">Pour définir l’environnement dans [Azure App Service](https://azure.microsoft.com/services/app-service/), effectuez les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="16ec8-299">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="16ec8-300">Sélectionnez l’application dans le panneau **App Services**.</span><span class="sxs-lookup"><span data-stu-id="16ec8-300">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="16ec8-301">Dans le groupe **PARAMÈTRES**, sélectionnez le panneau **Paramètres de l’application**.</span><span class="sxs-lookup"><span data-stu-id="16ec8-301">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="16ec8-302">Dans la zone **Paramètres d’application**, sélectionnez **Ajouter un nouveau paramètre**.</span><span class="sxs-lookup"><span data-stu-id="16ec8-302">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="16ec8-303">Pour **Entrer un nom**, spécifiez `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-303">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="16ec8-304">Pour **Entrer une valeur**, spécifiez l’environnement (par exemple `Staging`).</span><span class="sxs-lookup"><span data-stu-id="16ec8-304">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="16ec8-305">Cochez la case **Paramètre d’emplacement** si vous souhaitez que le paramètre d’environnement reste avec l’emplacement actuel quand des emplacements de déploiement sont permutés.</span><span class="sxs-lookup"><span data-stu-id="16ec8-305">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="16ec8-306">Pour plus d’informations, consultez [Documentation Azure : Quels sont les paramètres qui sont permutés ?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="16ec8-306">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="16ec8-307">Sélectionnez **Enregistrer** en haut du panneau.</span><span class="sxs-lookup"><span data-stu-id="16ec8-307">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="16ec8-308">Azure App Service redémarre automatiquement l’application après qu’un paramètre d’application (variable d’environnement) est ajouté, changé ou supprimé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="16ec8-308">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="16ec8-309">Portail</span><span class="sxs-lookup"><span data-stu-id="16ec8-309">Windows</span></span>

<span data-ttu-id="16ec8-310">Pour définir `ASPNETCORE_ENVIRONMENT` pour la session actuelle quand l’application est démarrée avec [dotnet run](/dotnet/core/tools/dotnet-run), les commandes suivantes sont utilisées :</span><span class="sxs-lookup"><span data-stu-id="16ec8-310">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="16ec8-311">**Invite de commandes**</span><span class="sxs-lookup"><span data-stu-id="16ec8-311">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="16ec8-312">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="16ec8-312">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="16ec8-313">Ces commandes prennent effet uniquement pour la fenêtre active.</span><span class="sxs-lookup"><span data-stu-id="16ec8-313">These commands only take effect for the current window.</span></span> <span data-ttu-id="16ec8-314">Quand la fenêtre est fermée, le paramètre `ASPNETCORE_ENVIRONMENT` reprend la valeur de machine ou la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="16ec8-314">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="16ec8-315">Pour définir la valeur globalement dans Windows, utilisez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="16ec8-315">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="16ec8-316">Ouvrez le **panneau** de configuration > **système** > **paramètres système avancés** et ajoutez ou modifiez la valeur de `ASPNETCORE_ENVIRONMENT` :</span><span class="sxs-lookup"><span data-stu-id="16ec8-316">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Propriétés système avancées](environments/_static/systemsetting_environment.png)

  ![Variable d’environnement ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="16ec8-319">Ouvrez une invite de commandes d’administration, puis utilisez la commande `setx`, ou ouvrez une invite de commandes PowerShell d’administration et utilisez `[Environment]::SetEnvironmentVariable` :</span><span class="sxs-lookup"><span data-stu-id="16ec8-319">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="16ec8-320">**Invite de commandes**</span><span class="sxs-lookup"><span data-stu-id="16ec8-320">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="16ec8-321">Le commutateur `/M` indique de définir la variable d’environnement au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-321">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="16ec8-322">Si le commutateur `/M` n’est pas utilisé, la variable d’environnement est définie pour le compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-322">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="16ec8-323">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="16ec8-323">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="16ec8-324">La valeur d’option `Machine` indique de définir la variable d’environnement au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-324">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="16ec8-325">Si la valeur d’option est remplacée par `User`, la variable d’environnement est définie pour le compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-325">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="16ec8-326">Quand la variable d’environnement `ASPNETCORE_ENVIRONMENT` est définie globalement, elle prend effet pour `dotnet run` dans n’importe quelle fenêtre Commande ouverte une fois la valeur définie.</span><span class="sxs-lookup"><span data-stu-id="16ec8-326">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="16ec8-327">**web.config**</span><span class="sxs-lookup"><span data-stu-id="16ec8-327">**web.config**</span></span>

<span data-ttu-id="16ec8-328">Pour définir la variable d’environnement `ASPNETCORE_ENVIRONMENT` avec *web.config*, consultez la section *Définition des variables d’environnement* à l’adresse <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-328">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="16ec8-329">**Fichier projet ou profil de publication**</span><span class="sxs-lookup"><span data-stu-id="16ec8-329">**Project file or publish profile**</span></span>

<span data-ttu-id="16ec8-330">**Pour les déploiements de Windows IIS :** Incluez la propriété `<EnvironmentName>` dans le [profil de publication (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) ou le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="16ec8-330">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="16ec8-331">Cette approche définit l’environnement dans *web.config* lorsque le projet est publié :</span><span class="sxs-lookup"><span data-stu-id="16ec8-331">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="16ec8-332">**Par pool d’applications IIS**</span><span class="sxs-lookup"><span data-stu-id="16ec8-332">**Per IIS Application Pool**</span></span>

<span data-ttu-id="16ec8-333">Pour définir la variables d’environnement `ASPNETCORE_ENVIRONMENT` pour une application qui s’exécute dans un pool d’applications isolé (prie en charge sur IIS 10.0 ou versions ultérieures), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="16ec8-333">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="16ec8-334">Quand la variable d’environnement `ASPNETCORE_ENVIRONMENT` est définie pour un pool d’applications, sa valeur remplace un paramètre au niveau du système.</span><span class="sxs-lookup"><span data-stu-id="16ec8-334">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16ec8-335">Lors de l’hébergement d’une application dans IIS et de l’ajout ou du changement de la variable d’environnement `ASPNETCORE_ENVIRONMENT`, utilisez l’une des approches suivantes pour que la nouvelle valeur soit récupérée par des applications :</span><span class="sxs-lookup"><span data-stu-id="16ec8-335">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="16ec8-336">Exécutez la commande `net stop was /y` suivie de `net start w3svc` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="16ec8-336">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="16ec8-337">Redémarrez le serveur.</span><span class="sxs-lookup"><span data-stu-id="16ec8-337">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="16ec8-338">macOS</span><span class="sxs-lookup"><span data-stu-id="16ec8-338">macOS</span></span>

<span data-ttu-id="16ec8-339">Vous pouvez définir l’environnement actuel pour macOS en ligne durant l’exécution de l’application :</span><span class="sxs-lookup"><span data-stu-id="16ec8-339">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="16ec8-340">Vous pouvez également définir l’environnement avec `export` avant d’exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="16ec8-340">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="16ec8-341">Les variables d’environnement de niveau machine sont définies dans le fichier *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="16ec8-341">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="16ec8-342">Modifiez le fichier à l’aide d’un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="16ec8-342">Edit the file using any text editor.</span></span> <span data-ttu-id="16ec8-343">Ajoutez l’instruction suivante :</span><span class="sxs-lookup"><span data-stu-id="16ec8-343">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="16ec8-344">Linux</span><span class="sxs-lookup"><span data-stu-id="16ec8-344">Linux</span></span>

<span data-ttu-id="16ec8-345">Pour les versions Linux, exécutez la commande `export` à une invite de commandes pour les paramètres de variable basés sur la session, et le fichier *bash_profile* pour les paramètres d’environnement de niveau machine.</span><span class="sxs-lookup"><span data-stu-id="16ec8-345">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="16ec8-346">Définir l’environnement dans le code</span><span class="sxs-lookup"><span data-stu-id="16ec8-346">Set the environment in code</span></span>

<span data-ttu-id="16ec8-347">Appelez <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> lors de la génération de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="16ec8-347">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="16ec8-348">Consultez <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-348">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="16ec8-349">Configuration par environnement</span><span class="sxs-lookup"><span data-stu-id="16ec8-349">Configuration by environment</span></span>

<span data-ttu-id="16ec8-350">Pour charger la configuration par environnement, nous vous recommandons de disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="16ec8-350">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="16ec8-351">fichiers *appSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="16ec8-351">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="16ec8-352">Consultez <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-352">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="16ec8-353">Variables d’environnement (définies sur chaque système sur lequel l’application est hébergée).</span><span class="sxs-lookup"><span data-stu-id="16ec8-353">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="16ec8-354">Consultez <xref:fundamentals/host/web-host#environment> et <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-354">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="16ec8-355">Secret Manager (dans l’environnement de développement uniquement).</span><span class="sxs-lookup"><span data-stu-id="16ec8-355">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="16ec8-356">Consultez <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="16ec8-356">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="16ec8-357">Classe et méthodes Startup en fonction de l’environnement</span><span class="sxs-lookup"><span data-stu-id="16ec8-357">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="16ec8-358">Injectez IHostingEnvironment dans Startup. configure</span><span class="sxs-lookup"><span data-stu-id="16ec8-358">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="16ec8-359">Injectez <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-359">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="16ec8-360">Cette approche est utile lorsque l’application requiert uniquement la configuration de `Startup.Configure` pour quelques environnements avec des différences de code minimales par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-360">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="16ec8-361">Injecter IHostingEnvironment dans la classe Startup</span><span class="sxs-lookup"><span data-stu-id="16ec8-361">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="16ec8-362">Injectez <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> dans le constructeur `Startup` et assignez le service à un champ à utiliser dans la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-362">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="16ec8-363">Cette approche est utile lorsque l’application requiert la configuration du démarrage pour quelques environnements avec des différences de code minimales par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-363">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="16ec8-364">Dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="16ec8-364">In the following example:</span></span>

* <span data-ttu-id="16ec8-365">L’environnement est conservé dans le champ `_env`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-365">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="16ec8-366">`_env` est utilisé dans `ConfigureServices` et `Configure` pour appliquer la configuration de démarrage en fonction de l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-366">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

### <a name="startup-class-conventions"></a><span data-ttu-id="16ec8-367">Conventions de la classe Startup</span><span class="sxs-lookup"><span data-stu-id="16ec8-367">Startup class conventions</span></span>

<span data-ttu-id="16ec8-368">Quand une application ASP.NET Core démarre, la [classe Startup](xref:fundamentals/startup) amorce l’application.</span><span class="sxs-lookup"><span data-stu-id="16ec8-368">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="16ec8-369">L’application peut définir des classes de `Startup` distinctes pour différents environnements (par exemple, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="16ec8-369">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="16ec8-370">La classe de `Startup` appropriée est sélectionnée au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="16ec8-370">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="16ec8-371">La classe dont le suffixe du nom correspond à l'environnement actuel est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="16ec8-371">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="16ec8-372">Si aucune classe `Startup{EnvironmentName}` correspondante n’est trouvée, la classe `Startup` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="16ec8-372">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="16ec8-373">Cette approche est utile lorsque l’application requiert la configuration du démarrage pour plusieurs environnements avec de nombreuses différences de code par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-373">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="16ec8-374">Pour implémenter des classes `Startup` basées sur l’environnement, créez une classe `Startup{EnvironmentName}` pour chaque environnement en cours d’utilisation et une classe `Startup` de base :</span><span class="sxs-lookup"><span data-stu-id="16ec8-374">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="16ec8-375">Utilisez la surcharge [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) qui accepte un nom d’assembly :</span><span class="sxs-lookup"><span data-stu-id="16ec8-375">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="16ec8-376">Conventions de la méthode Startup</span><span class="sxs-lookup"><span data-stu-id="16ec8-376">Startup method conventions</span></span>

<span data-ttu-id="16ec8-377">[Configurez](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) prennent en charge des versions spécifiques à l’environnement de la forme `Configure<EnvironmentName>` et `Configure<EnvironmentName>Services`.</span><span class="sxs-lookup"><span data-stu-id="16ec8-377">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="16ec8-378">Cette approche est utile lorsque l’application requiert la configuration du démarrage pour plusieurs environnements avec de nombreuses différences de code par environnement.</span><span class="sxs-lookup"><span data-stu-id="16ec8-378">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="16ec8-379">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="16ec8-379">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end